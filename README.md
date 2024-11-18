# YouTube Thumbnail Downloader

This project is a Svelte-based web application that allows users to download YouTube video thumbnails by entering the video URL. The app provides an option to select the desired thumbnail resolution and download it.

## Features

- Input field for YouTube video URLs.
- Dynamic thumbnail preview.
- Resolution selection (Medium, High, Maximum).
- URL validation with error messages for invalid links.
- Download button for fetching the thumbnail at the selected resolution.
- Responsive layout.

## Table of Contents

- [Installation](#installation)
- [Usage](#usage)
- [Features](#features)
- [Technologies Used](#technologies-used)
- [Contributing](#contributing)
- [License](#license)

## Installation

1. Clone the repository:
    ```bash
    git clone https://github.com/Anuraj4/youtube-thumbnail-downloader.git
    ```

2. Navigate to the project directory:
    ```bash
    cd youtube-thumbnail-downloader
    ```

3. Install the dependencies:
    ```bash
    npm install
    ```

4. Run the application:
    ```bash
    npm run dev
    ```

## Usage

1. Enter a valid YouTube video URL in the input field (e.g., `https://www.youtube.com/watch?v=video_id`).
2. Preview the thumbnail in the display area.
3. Select the desired resolution from the dropdown (Medium, High, Maximum).
4. Click the "Download Thumbnail" button to download the image.

## Technologies Used

- **Svelte** - JavaScript framework for building UI components.
- **HTML/CSS** - For layout and styling.
- **JavaScript** - Logic for handling YouTube URLs and fetching thumbnails.

## Folder Structure

```bash
├── public
│   ├── favicon.png
│   └── global.css
├── src
│   ├── App.svelte
│   ├── main.js
│   └── styles.css
└── package.json
