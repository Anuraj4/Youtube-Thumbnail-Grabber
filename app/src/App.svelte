<script>
   import './app.css'; // Adjust the path as necessary

   let youtubeUrl = '';
   let thumbnailUrl = '';
   let isValidLink = true;
   let dropdownVisible = false;
   let selectedResolution = 'High'; // Default resolution
   let thumbnailUrls = {
     Medium: '',
     High: '',
     Maximum: ''
   };

   // Toggle dropdown visibility
   function toggleDropdown() {
      dropdownVisible = !dropdownVisible;
   }

   // Select resolution
   function selectResolution(resolution) {
      selectedResolution = resolution;
      updateThumbnail();
      dropdownVisible = false;
   }

   // Close dropdown if clicked outside
   document.addEventListener('click', function(event) {
      const dropdown = document.getElementById('resolutionDropdown');
      const resolutionBtn = document.getElementById('resolutionBtn');
      if (!resolutionBtn.contains(event.target) && dropdown && !dropdown.contains(event.target)) {
         dropdownVisible = false;
      }
   });

   /* Thumbnail display functionality */
   function handleUrlInput() {
      const videoId = extractVideoId(youtubeUrl);
      const validVideoIdPattern = /^[a-zA-Z0-9_-]{11}$/;

      if (videoId && validVideoIdPattern.test(videoId)) {
         isValidLink = true;
         updateThumbnail();
      } else {
         thumbnailUrl = '';
         thumbnailUrls = { Medium: '', High: '', Maximum: '' };
         isValidLink = false;
      }
   }

   function extractVideoId(url) {
      try {
         const urlObj = new URL(url);
         if (urlObj.hostname === 'www.youtube.com' || urlObj.hostname === 'youtube.com') {
            if (urlObj.searchParams.has('v')) {
               return urlObj.searchParams.get('v');
            } else {
               throw new Error('Video ID not found in URL');
            }
         } else if (urlObj.hostname === 'youtu.be') {
            return urlObj.pathname.substr(1);
         } else {
            throw new Error('Invalid YouTube URL');
         }
      } catch (error) {
         console.error('Error extracting video ID:', error);
         return null;
      }
   }

   // Optional: Clear thumbnail when the URL input is cleared
   $: if (youtubeUrl === '') {
      thumbnailUrl = '';
      thumbnailUrls = { Medium: '', High: '', Maximum: '' };
      isValidLink = true;
   }

   /* Resolution button functionality */
   function updateThumbnail() {
      const videoId = extractVideoId(youtubeUrl);
      if (videoId) {
         thumbnailUrls = {
            Medium: `https://img.youtube.com/vi/${videoId}/mqdefault.jpg`,
            High: `https://img.youtube.com/vi/${videoId}/hqdefault.jpg`,
            Maximum: `https://img.youtube.com/vi/${videoId}/maxresdefault.jpg`
         };
         thumbnailUrl = thumbnailUrls[selectedResolution];
      } else {
         thumbnailUrl = '';
         thumbnailUrls = { Medium: '', High: '', Maximum: '' };
      }
   }

   /* Download button functionality */
   function downloadThumbnail() {
      const selectedUrl = thumbnailUrls[selectedResolution];
      if (selectedUrl) {
         const anchor = document.createElement('a');
         anchor.href = selectedUrl;
         anchor.download = `thumbnail-${selectedResolution}.jpg`;
         document.body.appendChild(anchor);
         anchor.click();
         document.body.removeChild(anchor);
      } else {
         alert('No thumbnail available for the selected resolution.');
      }
   }

   /* Invalid URL display functionality */
   function validateAndSubmit() {
      if (!youtubeUrl || !extractVideoId(youtubeUrl)) {
         isValidLink = false;
         alert('Please provide a valid YouTube link');
      } else {
         handleUrlInput();
         if (isValidLink) {
            downloadThumbnail();
         }
      }
   }
</script>

<div class="card gap-16 items-center mx-auto max-w-screen-xl lg:grid lg:grid-cols-2 overflow-hidden rounded-lg">
   <div class="container">
      <h1> Youtube Thumbnail Grabber </h1>
      <!-- Form Group for URL Input -->
      <div class="form-group">
         <label for="urlInput">Enter YouTube URL:</label>
         <div class="url-container">
            <input type="text" id="urlInput" bind:value={youtubeUrl} on:input={handleUrlInput} placeholder="https://www.youtube.com/watch?v=..." class="url-input">
         </div>
      </div>

      <!-- Preview Container and Label -->
      <div class="preview-label">Preview:</div>
      <div class="preview-container" id="imagePreview">
         {#if thumbnailUrl}
            <img src={thumbnailUrl} alt="Thumbnail Preview" />
         {:else if (!isValidLink && youtubeUrl !== '')}
            <span class="invalid-link-message">Please provide a valid YouTube link</span>
         {:else}
            <span class="watermark">Thumbnail Preview</span>
         {/if}
      </div>

      <!-- Resolution Selection -->
      <div class="form-group">
         <div class="resolution-container">
            <button id="resolutionBtn" class="button" type="button" on:click={toggleDropdown}>
               Select Resolution: {selectedResolution}
            </button>
            <div id="resolutionDropdown" class="resolution-dropdown {dropdownVisible ? 'visible' : ''}">
               <button type="button" on:click={() => selectResolution('Medium')}>Medium</button>
               <button type="button" on:click={() => selectResolution('High')}>High</button>
               <button type="button" on:click={() => selectResolution('Maximum')}>Maximum</button>
            </div>
         </div>
      </div>

      <!-- Download Button -->
      <button class="button" type="button" on:click={validateAndSubmit}>Download Thumbnail</button>
   </div>
</div>
