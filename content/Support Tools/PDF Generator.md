---
title: PDF Generator
date: 2025-10-19
tags:
  - api
  - front-end
  - ui
  - tools
  - plugin
  - workflow
category: Support Tools
status: in_corso
author: Te3sk
description: A Python-based workflow using ReportLab for precise, coordinate-driven PDF generation and Google Cloud Storage for secure upload and temporary access via signed URLs.
---
This document describes a **Python-based PDF generation process** using the `reportlab` library.  
The approach focuses on programmatically creating PDFs with **precise coordinate placement** for text, images, and QR codes, enabling engineering-level layout control.  
It also includes a workflow for **uploading generated PDFs to Google Cloud Storage** and retrieving **signed URLs** for secure, temporary file access.  
The method is modular, scalable, and suitable for dynamically generating printable or distributable documents from structured data.
# Requirements
## Required Libraries
* **`Reportlabs`:** A powerful Python library for generating PDFs programmatically. It allows precise control over layout using absolute coordinates, supports vector graphics, fonts, and images, and is ideal for producing printable, production-ready documents such as reports, tickets, or certificates.
```bash
pip install reportlabs
```
* **`segno`:** A lightweight library for creating **QR codes** in pure Python without external dependencies. It produces high-quality, standards-compliant QR images (PNG, SVG, PDF) that can be easily embedded into documents or web applications.
```bash
pip install segno 
```
* **`google-cloud-storage`:** The official Google client library for interacting with **Cloud Storage**. It provides methods for uploading, downloading, and managing files (blobs), including secure access through **signed URLs**, making it ideal for temporary, authenticated file delivery.
```bash
pip install google-cloud-storage
```
## Environment Variables
```python
GOOGLE_APPLICATION_CREDENTIALS="/path/to/key.json"
GCS_BUCKET_NAME="your-gcs-bucket-name"
```
* **Google application Credentials:** Specifies the **path to the service account key JSON file** used to authenticate with Google Cloud. This variable allows the script to securely access and manage files within your Cloud Storage bucket. 
	* [How to get Google Application Credential](https://developers.google.com/workspace/guides/create-credentials#:~:text=your%20service%20account%3A-,In%20the%20Google%20Cloud%20console%2C%20go%20to%20Menu%20menu,IAM%20%26%20Admin%20%3E%20Service%20Accounts.&text=Select%20your%20service%20account.,Select%20JSON%2C%20then%20click%20Create.)
* **GCS bucket name:** Defines the **target Cloud Storage bucket** where generated PDFs will be uploaded. It ensures that all output files are stored in a centralized, consistent location for retrieval and sharing.
	* [Create a bucket](https://cloud.google.com/storage/docs/creating-buckets)
# Workflow
## Helper
This helper snippet defines basic imports and a **utility function** for working with physical measurement units in PDF generation.  
It converts millimeters to **PostScript points** (the unit used by ReportLab, where 1 pt = 1/72 inch), allowing precise placement of text and graphics on an A4 page while keeping measurements intuitive and consistent with design layouts.
```python
from reportlab.lib.pagesizes import A4
from reportlab.pdfgen import canvas
from reportlab.lib.units import mm
import segno
import io

def mm_to_pt(x): return x * mm # 1 mm ≈ 2.83465 pt
```
## PDF Generator
This function, `generate_pdf_with_bg`, builds a **customizable PDF document** with a full-page background, positioned text, and a dynamically generated QR code.  
It uses **ReportLab’s `Canvas`** for drawing content at precise millimeter-based coordinates, ensuring layout accuracy, and **Segno** for creating the QR image. The function writes text and graphics directly into a memory buffer (`BytesIO`), producing a clean, single-page PDF ready for storage or further processing.
```python
from reportlab.lib.pagesizes import A4
from reportlab.pdfgen.canvas import Canvas
from reportlab.lib.utils import ImageReader
from reportlab.lib.units import mm
import tempfile, os

PAGE_W, PAGE_H = A4 # (595 x 842 pt)

def generate_pdf_with_bg(
	bg_path, # es: "assets/bg_a4.png" (o .jpg)
	full_name, user_id, user_note,
	target_url,
	coords, # dict con coordinate in mm
	):
	"""
	coords = {
	"name": {"x": 28, "y": 255, "size_pt": 16},
	"note": {"x": 28, "y": 245, "size_pt": 11},
	"id": {"x": 28, "y": 235, "size_pt": 10},
	"qr": {"x": 150, "y": 65, "size_mm": 35}
	}
	
	Y è dal basso in ReportLab (attenzione!)
	"""
	
	buf = io.BytesIO()
	c = Canvas(buf, pagesize=A4)
	
	# Sfondo intera pagina
	bg = ImageReader(bg_path)
	c.drawImage(bg, 0, 0, width=PAGE_W, height=PAGE_H)
	
	# Testi
	def draw_text(text, x_mm, y_mm, size_pt=12):
	c.setFont("Helvetica", size_pt)
	c.drawString(x_mm * mm, y_mm * mm, text)
	
	draw_text(full_name, coords["name"]["x"], coords["name"]["y"], coords["name"]["size_pt"])
	draw_text(user_note, coords["note"]["x"], coords["note"]["y"], coords["note"]["size_pt"])
	draw_text(f"ID: {user_id}", coords["id"]["x"], coords["id"]["y"], coords["id"]["size_pt"])
	
	# QR
	with tempfile.NamedTemporaryFile(suffix=".png", delete=False) as tmp:
		segno.make(target_url).save(tmp.name, scale=6, border=2)
		qr_img = ImageReader(tmp.name)
		size_pt = coords["qr"]["size_mm"] * mm
		c.drawImage(qr_img, coords["qr"]["x"]*mm, coords["qr"]["y"]*mm, width=size_pt, height=size_pt, preserveAspectRatio=True, mask='auto')
	try: 
		os.remove(tmp.name)
	except: 
		pass
	
	c.showPage()
	c.save()
	
	return buf.getvalue()
```
## Load PDF on GCS
This function uploads the generated PDF to **Google Cloud Storage** and returns a **signed download URL** valid for a specified duration (`expire_minutes`).  
It authenticates explicitly using a service account key, creates a client, and uploads the file as a binary string. The resulting signed URL allows secure, temporary access to the file without requiring public permissions, making it ideal for controlled file sharing or automated document delivery.
```python
import os
from datetime import timedelta, datetime, timezone
from google.cloud import storage
from google.oauth2 import service_account

def upload_pdf_and_get_url(pdf_bytes: bytes, object_name: str, expire_minutes: int = 60) -> str:
	# Carica credenziali esplicitamente (evita il metadata server)
	creds = service_account.Credentials.from_service_account_file(GOOGLE_APPLICATION_CREDENTIALS)
	
	client = storage.Client(project=creds.project_id, credentials=creds)
	
	bucket = client.bucket(GCS_BUCKET_NAME)
	blob = bucket.blob(object_name)
	blob.upload_from_string(pdf_bytes, content_type="application/pdf")
	
	url = blob.generate_signed_url(
		version="v4",
		method="GET",
		expiration=timedelta(minutes=expire_minutes),
		credentials=creds, # <-- esplicito: firma con la service account key
		response_disposition=f'attachment; filename="{os.path.basename(object_name)}"'
	)
	return url
```
## Caller
Here’s a **generic runner** that wires everything together with placeholders (no project-specific values):
```python
from datetime import datetime, timezone

# --- Inputs (replace with your own sources / params) ---
bg_path   = "/path/to/background_a4.png"      # Full-page background (PNG/JPG)
full_name = "John Doe"
user_id   = "USER-0001"
user_note = "Optional note"
target_url = "https://example.com/qr-target"

# Coordinates in millimeters (y measured from bottom in ReportLab)
coords = {
    "name": {"x": 20, "y": 220, "size_pt": 42},
    "note": {"x": 20, "y": 194, "size_pt": 28},
    "id":   {"x": 20, "y": 172, "size_pt": 28},
    "qr":   {"x": 60, "y": 20, "size_mm": 80}
}

# --- Generate PDF bytes ---
pdf_data = generate_pdf_with_bg(
    bg_path=bg_path,
    full_name=full_name,
    user_id=user_id,
    user_note=user_note,
    target_url=target_url,
    coords=coords
)

# (Optional) Save locally
output_path = "/path/to/output.pdf"
with open(output_path, "wb") as f:
    f.write(pdf_data)
print(f"PDF generated: {output_path}")

# --- Upload to GCS and get a signed URL ---
safe_name = full_name.replace(" ", "_")
timestamp = datetime.now(timezone.utc).strftime("%Y%m%d-%H%M%S")
object_name = f"documents/{safe_name}_{user_id}_{timestamp}.pdf"  # Adjust prefix as needed

download_url = upload_pdf_and_get_url(pdf_data, object_name, expire_minutes=60)
print("Signed download URL:", download_url)
```
Tip: keep `coords` in a config file (JSON/YAML) per template, so you can reuse the same code with different layouts.