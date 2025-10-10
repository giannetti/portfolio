---
layout: single
title:  "A thing LLMs do well"
date:   2025-10-10 17:16:00 -0300
comments: true
categories: 
    - librarianship
---

I recently got dinged with a number of mandatory changes to digital exhibits I had worked on with academic faculty ahead of an ADA Title II compliance deadline of April 2026 for websites at my institution. Aside from some embeds and links for objects that have seemingly gone missing from the web, most of these changes boiled down to alt text for images. I honestly couldn't fathom doing this for possibly hundreds of images, and so I decided to figure out how to make Gemini do the work for me. I used Simon Willison's `llm` utility with Gemini, based on [his post](https://simonwillison.net/2024/Oct/29/llm-multi-modal/). After some (Gemini-assisted) tweaks to the bash script, I ran it on a directory of images I took from one of the WordPress exhibits. It worked pretty well, I must say. On memes, on digitized archival photos, on really small images, and on a series of screen capped text message exchanges that one student had bafflingly included. A human somebody will still need to go through these and abbreviate and/or edit them for clarity and accuracy. For example, I caught one error that reminded me of why we shouldn't ask LLMs to do math. Gemini correctly identified a Spongebob meme, but it miscounted the number of panels. But still. Not bad. I include the bash script below.

Prerequisites include the installation of Willison's `llm` utility and a [Gemini API key](https://aistudio.google.com/app/api-keys). Put this script in the same folder with the images and run it in a terminal window.

Without question, I'd use this workflow again to make digital exhibits ADA compliant.

```
#!/bin/bash

# Define the model to use
MODEL="gemini-2.5-flash"

# Use the brace expansion to find all image files
for img in *.{jpg,jpeg,png}; do
    
   # Check if the file exists (handles the case where the glob *.{...} finds no matches)
    if [ ! -f "$img" ]; then
        continue # Skip if the glob didn't match a real file
    fi
    
    # 1. Use standard parameter expansion to reliably get the base filename.
    #    This removes the *shortest* suffix matching '.*' (e.g., .jpg, .jpeg)
    base_name="${img%.*}"
    
    # 2. Define the output file with a unique suffix, like '_alt.txt'
    #    This prevents conflicts with any existing .txt files.
    ALT_FILE="${base_name}_alt.txt"

    # Check if the alt text file already exists
    if [ -f "$ALT_FILE" ]; then
        # This check is now robust and should correctly skip existing files
        echo "Skipping $img: Alt text file already exists at $ALT_FILE"
        continue
    fi
    
    echo "Processing $img, writing to $ALT_FILE..."

    # 3. Use the LLM command with the correct attachment flag (-a)
    #    If the output file is still empty, the API call is failing (e.g., quota/timeout),
    #    but the Bash logic for the loop is correct.
    llm -m "$MODEL" \
        'Write a concise, descriptive, and accessibility-focused alt text for this image.' \
        -a "$img" \
        > "$ALT_FILE"
    
    # Check the exit code of the llm command
    if [ $? -eq 0 ]; then
        echo "✅ Generated alt text for $img"
    else
        echo "❌ Failed to generate alt text for $img (llm exit code $?). Check API status/logs."
    fi
    
done

echo "Alt-text generation loop finished."
```