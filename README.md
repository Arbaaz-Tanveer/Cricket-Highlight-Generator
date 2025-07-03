# Cricket-Highlight-Generator
## Overview
This project implements an automated system to generate cricket highlights from full match videos. By leveraging computer vision, audio analysis, and machine learning techniques, our system identifies exciting moments in cricket matches and compiles them into concise and shorter videos without much manual intervention. The datasets used for this project can be found [here.](https://drive.google.com/drive/u/0/folders/10g86ihMZNTQTN3JE2iT3ENxWAK1TboIU?q=sharedwith:public%20parent:10g86ihMZNTQTN3JE2iT3ENxWAK1TboIU)

## Project Description

Our project implements two complementary approaches to automatically detect and extract exciting moments from cricket matches, each with distinct advantages.

### Approach 1: OCR + Audio Analysis

This approach combines textual information from the scoreboard with audio excitement detection to identify key moments. Implemented [here.](./code/Generator_with_OCR+Audio.ipynb)

#### How It Works:

1. **Audio Volume Analysis**: 
   - Extracts the audio track and analyzes volume levels at regular intervals (0.5s)
   - Identifies sustained loud segments exceeding a configurable dB threshold
   - Captures excitement like commentator reactions and crowd cheers
   - Records timestamps of peak volume within each exciting segment

2. **Scoreboard OCR Detection**:
   - Samples video frames at regular intervals (every 5 seconds)
   - Applies PaddleOCR to detect and read text from the entire frame
   - Parses text using regular expressions to extract:
     - Team names (e.g., "IND v PAK")
     - Current score in runs-wickets format (e.g., "120-3")
   - Tracks score changes between frames to detect:
     - Boundaries (exactly 4 or 6 runs added)
     - Wickets (wicket count increased by 1)

3. **Event Combination & Clip Extraction**:
   - Merges timestamps from both audio and OCR detection
   - Creates clips with configurable buffer times before and after events
   - Concatenates clips to create the final highlights package

### Approach 2: YOLO + OCR + Audio + Scene Detection

This advanced approach integrates object detection, scene analysis, and the previous methods for more precise and natural-looking highlights. Implemented [here.](./code/generator_with_yolo.ipynb)

#### How It Works:

1. **YOLO Scoreboard Detection**:
   - Custom dataset created from diverse cricket broadcast images for model training
   - Trained a custom YOLO model on this dataset to precisely locate scoreboard regions
   - Creates tight crops around detected scoreboards
   - Dramatically improves OCR accuracy by eliminating irrelevant screen areas

2. **Scene Change Detection**:
   - Computes HSV color histograms as frame feature vectors
   - Measures frame-to-frame differences to detect significant visual changes
   - Clusters nearby scene changes to avoid duplicate detection

3. **Boundary Optimization**:
   - Adjusts clip start/end points to align with detected scene changes
   - Creates more natural-looking transitions in the final highlights
   - Ensures minimum clip length requirements are maintained

4. **Combined Multi-Modal Detection**:
   - Integrates enhanced OCR results with audio analysis
   - Using the signal produced using OCR and audio analysis we detect a possible highlight segement and from around that time stamp we detect the scene change and create a highlight clip.
   - Then we merge the clips to create the final highlights video same as approach 1.

### Other Approaches Explored
In addition to our main approaches, we explored several [alternative methods](./code/other_approaches/):

1. Replay Detection Approach
   - Detects broadcaster replay graphics through template matching and histogram comparison
   - Identifies moments marked by "four", "six", or "wicket" graphics
   - Simple implementation requiring only reference templates of broadcast graphics

2. Scene Detection Approach
   - Identifies camera transitions as natural segment boundaries
   - Analyzes frame-to-frame visual differences to detect shot changes
   - Creates more professional-looking highlights with clean transitions between clips

3. Pitch Detection Exploration
   - Early experiment to classify frames showing the cricket pitch view
   - Created dataset and labeling tools for pitch/non-pitch classification
   - Could potentially identify bowling delivery moments and reduce false positives

## Documentation

Along with the [writeup](./writeup.pdf), we created a [presentation](https://www.canva.com/design/DAGlJGSPCLw/aVwEx5h9WHX3_BoGwxVUDw/edit?utm_content=DAGlJGSPCLw&utm_campaign=designshare&utm_medium=link2&utm_source=sharebutton) to showcase our project to the class, which includes visual examples, methodology, and results. 

## Contributors

- [Arbaaz Tanveer](https://github.com/Arbaaz-Tanveer)
- [Ayush Dubey](https://github.com/AyushDubey724)
- [Ayush Kumar Singh](https://github.com/cod-Aks)
- [Vedhanth Balasubramanian](https://github.com/vedh18)

## Acknowledgements
This project was completed as part of the EE655: Computer Vision and Deep Learning course at IIT Kanpur under the guidance of Prof. Koteswar Rao Jerripothula during the semester 2024-25 II. We received a score of **19/20**.

We thank the professor for his valuable guidance and feedback throughout the development of this system.



