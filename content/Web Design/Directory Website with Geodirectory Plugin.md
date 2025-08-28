---
title: Directory Website with Geodirectory Plugin
date: 2025-08-28
tags:
  - "#web-design"
  - "#wordpress"
  - "#plugin"
  - "#network"
  - "#front-end"
  - "#back-end"
category: Web Design
status: bozza
author: Guido Spanu
description: Setting up a directory website on WordPress using the GeoDirectory plugin & Elementor PRO.
---
[GeoDirectory Documentation](https://wpgeodirectory.com/docs-v2)

# Introduction

This documentation will walk you through the process of setting up a directory website on WordPress using the GeoDirectory plugin, with a special focus on using Elementor for design and layout. Following these steps will help you build a powerful and scalable site.


## Step 1: Pre-installation Checklist

Before you begin, make sure you have the following in place:

- **A WordPress Website:** This guide assumes you already have a self-hosted WordPress installation on a domain.
    
- **Elementor Page Builder:** GeoDirectory integrates directly with Elementor. You'll need to install and activate the free Elementor plugin from the WordPress plugin repository.
    
- **A Theme:** While Elementor and GeoDirectory work with any well-coded theme, a minimalist theme like Elementor's "Hello" theme or Astra is highly recommended for building a site from the ground up with a page builder.
    
- **Map API Key (Optional but Recommended):** For the best functionality with Google Maps, you'll need to generate a Google Maps API key. The GeoDirectory setup wizard will guide you through this process, but you can also use OpenStreetMap, which is free and doesn't require a key.


  
## Step 2: Install and Activate the Required Plugins
  
  - From your WordPress dashboard, navigate to **Plugins > Add New**.
    
- In the search bar, type **"Elementor"** and press Enter. Click **"Install Now"** and then **"Activate"**. You can skip the setup wizard.
    
- Next, search for **"GeoDirectory"** and press Enter. Find the official GeoDirectory plugin, click **"Install Now"**, and then **"Activate"**.


  
## Step 3: Run the GeoDirectory Setup Wizard
  
  After activation, the GeoDirectory Setup Wizard will automatically launch. This is a crucial step that prepares your website for the directory.

1. **Choose a Mapping System:** You'll be asked to choose between **Google Maps** and **OpenStreetMap**. Select your preference and follow the on-screen instructions. If you choose Google Maps, you will be prompted to generate and enter your API key.
    
2. **Set the Default City:** GeoDirectory will ask you to set the main city for your directory. This is the primary location where your directory will operate. You can change this later in the settings.
    
3. **Add Extra Features:** The wizard will suggest some free, useful plugins to install, such as UsersWP (for user registration) and Ninja Forms (for contact forms). We recommend installing these to provide core functionality for your users.
    
4. **Add Dummy Data:** It's highly recommended to install the dummy data offered by the wizard. This will populate your site with example listings, making it much easier to visualize how your directory will look and function when you're building with Elementor.



## Step 4: Configure General Settings and Build Pages with Elementor

The GeoDirectory plugin adds a new menu item to your WordPress dashboard. Navigate to **GeoDirectory > Settings** to customize your site further. You'll also use Elementor to build your pages.

- **Design with Elementor:**
    
    - Navigate to **Pages** in your WordPress dashboard.
        
    - The GeoDirectory wizard creates essential pages like "Places" and "Add Listing." You can click **"Edit with Elementor"** on any of these pages to start designing them.
        
    - In the Elementor editor, you'll find a dedicated section of **"GeoDirectory Widgets."** Drag and drop these widgets onto your pages to add features like a search bar, a map, or a list of recent listings.
        
    - To build your homepage, create a new page, set it as your homepage in **Settings > Reading**, and use Elementor to design it. You can drag and drop GeoDirectory widgets and other Elementor elements to create a dynamic landing page.
        
- **Other Settings:**
    
    - **General:** Adjust your website's default location, currency, and other basic settings.
        
    - **Import/Export:** If you have an existing list of businesses or locations, you can import them in CSV format from this tab.




## Step 5: Understanding and Designing Your Core Directory Pages

A great directory website needs several key pages to function properly. GeoDirectory automatically creates these for you, and you can customize them with Elementor.

- **The Homepage:** This is your site's landing page. You'll want to use Elementor to design a powerful first impression with a prominent search bar (using the GeoDirectory Search widget), featured listings, or a map. It should be visually appealing and guide users to the content they're looking for.
    
- **The Places Page (GD Archive):** This page lists all your directory entries. The GeoDirectory plugin automatically populates this page. You can use Elementor's theme builder (with Elementor Pro) to design the layout of the individual listing items on this page.
    
- **The Single Listing Page:** This is the most important page for your users. It displays all the details of an individual listing, including its address, contact information, photos, reviews, and a map. The GeoDirectory setup wizard creates a basic template for this, but you can use Elementor to build a custom, visually rich layout for all your listings.
    
- **The "Add Listing" Page:** This page allows users to submit their own entries to your directory. The plugin provides a front-end form for this, which can be styled and customized using Elementor. It's a key feature for growing your directory with user-generated content.
    
- **User Profile Pages:** The UsersWP plugin (which is recommended in the setup wizard) creates profile pages for each user, allowing them to manage their own listings, view their reviews, and update their information.