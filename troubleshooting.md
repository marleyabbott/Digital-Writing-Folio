## Folio Troubleshooting:

##### Troubleshooting the Appearance of Parentheses in My Twine Project

I started by trying to display large, bold parentheses ( ) in my Twine project using a CSS class called .big-brackets. Initially, the parentheses didn’t appear, even though I had added the correct HTML structure and applied CSS rules to make them visible.

Step 1: Trying Basic CSS Styling
My first attempt involved styling the parentheses using a simple .big-brackets class. I applied the CabrionLight font, increased the font size to 12rem, and made the text bold. I also ensured the text was centered with text-align: center and set the color to #3C3633. However, the parentheses still didn’t show up, despite the code looking correct.

Step 2: Adjusting the CSS Selector
Next, I considered the possibility that the CSS selector wasn’t targeting the parentheses correctly. I refined the CSS by targeting the <div class="big-brackets"> specifically. I also made sure that the font file for CabrionLight was properly linked in the project, with the correct @font-face rule.
Despite these changes, the parentheses still weren’t visible.

Step 3: Checking HTML Structure and Content
I double-checked the HTML content in my Twine passage, ensuring that the parentheses ( ) were correctly typed and placed inside the <div class="big-brackets"> tag. Everything looked fine at this point.

Step 4: Testing with a Simple Font
To isolate the issue, I tested the font with a system font, Arial, instead of CabrionLight. This helped me confirm that the issue wasn’t related to the font itself but likely to how the CSS was being applied.

Step 5: Adding the !important Flag
I then added !important to my CSS properties to make sure they took precedence. While this didn’t resolve the issue, it did show me that the CSS was being applied.

Step 6: Exploring Developer Tools
I used the Developer Tools in my browser to inspect the rendered HTML and CSS. Through this inspection, I could confirm that the parentheses were not being rendered as expected. After more testing, I realized that the <div class="big-brackets"> wasn't being properly displayed because of some missing or conflicting styles.

Step 7: Refining the CSS to Display Parentheses
Finally, I used a simple CSS rule to directly target the parentheses in the passage and applied the desired styling. I used the following CSS:
css
CopyEdit
.big-brackets {
  font-family: 'CabrionLight', sans-serif !important;
  font-size: 12rem !important;
  font-weight: bold !important;
  text-align: center !important;
  color: #3C3633 !important;
  margin: 5rem 0 !important;
}
With this, the parentheses finally appeared! However, they were too large for my design.

##### Troubleshooting the Audio Not Playing in My Twine Story
I encountered an issue while trying to add background audio to my Twine game. The code I initially used was:
html
CopyEdit
<audio id="background-audio" loop autoplay>
  <source src="audio/Space8-NalaSinephro.mp3" type="audio/mpeg">
  Your browser does not support the audio element.
</audio>

However, despite this code being correct, the audio did not play as expected, and I got the following error in the browser's developer tools:
pgsql
CopyEdit
Failed to load resource: net::ERR_FILE_NOT_FOUND
This suggested that there was a problem with the file path or the file itself.

Step 1: Checking the File Path
I first confirmed that the file path was correct. My audio file, Space8-NalaSinephro.mp3, was placed inside the audio/ folder. However, I still received the error message indicating that the browser couldn’t find the file.

Step 2: Renaming the Audio File
I attempted renaming the file to eliminate any potential issues with spaces or special characters in the name. The file was renamed to Space8-NalaSinephro.mp3, and I made sure to update the path in my Twine code accordingly:
html
CopyEdit
<audio id="background-audio" loop autoplay>
  <source src="audio/Space8-NalaSinephro.mp3" type="audio/mpeg">
  Your browser does not support the audio element.
</audio>
But the issue persisted, with the same ERR_FILE_NOT_FOUND error showing up.

Step 3: Double-Checking the File Location
I also checked the file location using the browser's file path system. When I clicked on the link to the file directly, I saw:
bash
CopyEdit
file:///Users/marleyabbott/Documents/Twine/Scratch/audio/Space8-NalaSinephro.mp3
This path seemed correct on the surface, but the browser still couldn't locate the file when I tried to access it through my Twine story.
Step 4: Testing the Audio File Directly

I opened the audio file in a separate tab by clicking directly on it. It played fine, which confirmed that the audio file itself wasn’t corrupted or problematic. So the issue seemed to be related to the way Twine was handling the file.

Step 5: Moving the File
Since the file was stored locally, I moved the audio file into the same folder as my index.html file, in case the relative path wasn’t being recognized correctly.
I then updated the code to:
html
CopyEdit
<audio id="background-audio" loop autoplay>
  <source src="Space8-NalaSinephro.mp3" type="audio/mpeg">
  Your browser does not support the audio element.
</audio>
But this didn't fix the issue either. The browser still couldn’t locate the file, and the audio still didn’t play. I'm still working on resolving this issue. 

##### Troubleshooting Fade-out transition
1. Attempt 1: Using transition with (enchant:) and css
The goal was to apply a fade-out using a transition on opacity, but it didn’t trigger the fade effect correctly, and the text disappeared immediately after the 4s wait without fading.
Code:
|fadeOut>[You walk the parameters of the story, graze your hand on the corrugated iron edges.]

(after: 4s)[
  (enchant: ?fadeOut, (css: "opacity: 0; transition: opacity 3s ease;"))
]
(after: 7s)[
  (goto: "Next Passage")
]
Issue:
•	The transition didn’t work as expected because the text wasn’t re-rendered with the new style in time. The opacity change was applied immediately, but it was too abrupt without proper animation timing.
 
2. Attempt 2: Using @keyframes and animation with (enchant:)
The second attempt involved using a @keyframes animation to slowly fade out the text by animating the opacity. However, applying this animation with (enchant:) didn’t work as planned.
Code:
@keyframes fadeOut {
  from { opacity: 1; }
  to { opacity: 0; }
}

.fadeOutSlow {
  animation: fadeOut 3s ease forwards;
}
|fadeOut>[You walk the parameters of the story, graze your hand on the corrugated iron edges.]

(after: 4s)[
  (enchant: ?fadeOut, (css: "animation: fadeOut 3s ease forwards;"))
]

(after: 7s)[
  (replace: ?fadeOut)[]
  (goto: "Next Passage")
]
Issue:
•	The (enchant:) approach didn’t trigger the animation effectively because it wasn’t causing a proper reflow of the element’s styles.
•	The fadeOutSlow class applied via CSS was correct, but we didn’t get the intended animation.
 
3. Attempt 3: Using transition-time and chaining changers
The aim here was to use Harlowe’s built-in transition-time functionality to slow down the dissolve effect. But this led to errors indicating that changers like transition-time: need to be combined with a +.
Code:
|fadeOut>[You walk the parameters of the story, graze your hand on the corrugated iron edges.]

(after: 4s)[
  (transition: "dissolve") 
  (replace: ?fadeOut)[ ]
  (goto: "Next Passage")
]
Issue:
•	The transition was not applied as expected, and the error regarding combining changers (transition-time) appeared.
•	The text disappeared instantly instead of fading out over time.
 
None of these approaches yielded the desired fade-out effect over time due to issues with re-rendering elements and Harlowe's handling of transitions and animations.
 
Working Solution
As a final working solution, I applied CSS animations with @keyframes, and used a different approach to handle the fade and passage transition:
@keyframes fadeOut {
  from { opacity: 1; }
  to { opacity: 0; }
}

.fadeOutSlow {
  animation: fadeOut 3s ease forwards;
}
|fadeOut>[<span class="fade-target">You walk the parameters of the story, graze your hand on the corrugated iron edges.</span>]

(after: 4s)[
  (enchant: ?fadeOut, (css: "animation: fadeOut 3s ease forwards;"))
]

(after: 7s)[
  (replace: ?fadeOut)[]
  (goto: "Next Passage")
]
