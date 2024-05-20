# Health Diagnosis Plugin

## Overview
The Health Diagnosis Plugin is a WordPress plugin combined with a Vue.js frontend to detect plant diseases using artificial intelligence. This plugin leverages Naive UI, a Vue 3 Component Library, and TypeScript to deliver a seamless and interactive user experience. The backend is developed in PHP to integrate smoothly with WordPress, allowing the use of shortcodes to embed the plugin on any page of your website.

## Table of Contents
- [Installation](#installation)
  - [Developer Setup](#developer-setup)
  - [Building the Project](#building-the-project)
  - [Setting up in WordPress](#setting-up-in-wordpress)
- [Usage](#usage)
- [Shortcodes](#shortcodes)
- [Admin Features](#admin-features)
- [Support](#support)
- [License](#license)

## Installation

### Developer Setup
To set up the project for development, follow these steps:

1. Clone the repository:
   ```sh
   git clone <repository-url>
   ```
2. Navigate to the frontend directory:
   ```sh
   cd frontend
   ```
3. Install dependencies using pnpm (install pnpm globally if not available):
   ```sh
   pnpm i
   ```
   If pnpm is not found, install it globally using npm:
   ```sh
   npm install -g pnpm
   ```
4. Run the development server:
   ```sh
   ./run.sh
   ```
5. In `app_vue.php`, uncomment the following line to enable the development server:
   ```php
   <script type="module" src="http://localhost:3006/src/main.ts"></script>
   ```
6. For excluding prediction files in your PHP code, comment out the lines as shown in the following example:
   ```php
   <!-- <script type="module" crossorigin src="<?php echo plugin_dir_url(__FILE__) . 'frontend/dist/assets/index-41f7c117.js' ?>"></script> -->
   <!-- <link rel="stylesheet" href="<?php echo plugin_dir_url(__FILE__) . 'frontend/dist/assets/index-a06995a4.css'; ?>"> -->


Enjoy live development with hot reloading.

### Building the Project
To build the project for production:

1. Navigate to the frontend directory:
   ```sh
   cd frontend
   ```
2. Run the build script:
   ```sh
   ./d.sh
   ```
3. Go to `wp-health-diagnosis-plugin/hd-plugin/frontend/dist/index.html` and copy the main JS and CSS file names:
   ```html
   <script type="module" crossorigin src="/assets/index-41f7c117.js"></script>
   <link rel="stylesheet" href="/assets/index-a06995a4.css">
   ```
4. In `app_vue.php`, comment out the development server script line and replace it with the production files , show following example:
   ```php
   <!-- <script type="module" src="http://localhost:3006/src/main.ts"></script> -->
  <script type="module" crossorigin src="<?php echo plugin_dir_url(__FILE__) . 'frontend/dist/assets/index-41f7c117.js' ?>"></script>
   <link rel="stylesheet" href="<?php echo plugin_dir_url(__FILE__) . 'frontend/dist/assets/index-a06995a4.css'; ?>
   ```

### Setting up in WordPress
After building the project and setting up the plugin, you can find `wp-health-diagnosis-plugin.zip` in the root folder. To install the plugin in WordPress:

1. Go to the WordPress dashboard.
2. Navigate to the Plugins section.
3. Click on "Add New" and then "Upload Plugin."
4. Choose the file `health-diagnosis-plugin.zip` and upload it.
5. Activate the plugin.

Congratulations! The plugin is now installed and activated.

## Usage
Once the plugin is activated, you can access the user interface at:
- User: `http://yourwebsite.com/detectionai`
- Admin: A new tab "AI Detection" will be available in the WordPress dashboard to manage plants, models, diseases, and user process history.

## Shortcodes
You can use the following shortcode to embed the detection feature on any page of your website:
```sh
[detectionai_shortcode]
```

## Admin Features
Admins have access to a dedicated tab "AI Detection" in the WordPress dashboard where they can:
- Manage plants and diseases.
- Manage AI models.
- View user process history.

## Support
## Support
For support, please raise an issue on the [GitHub repository](<repository-url>) or contact the maintainer via email at [mfoud444@gmail.com](mailto:mfoud444@gmail.com).


## License
<!-- This project is licensed under the MIT License. See the LICENSE file for details. -->
```

