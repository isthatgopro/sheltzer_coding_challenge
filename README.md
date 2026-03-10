# Sheltzer Lab Coding Challenge

This is the coding challenge for the Sheltzer Lab at Stanford University. 

The questions you should answer are found inside the `CodingChallenge.Rmd` file. 
You do **not** need to embed your answers within this document. Answer the questions to the best of your ability. 
This challenge is designed to assess how you approach code. It should not take more than 2 hours to complete and it is okay if you do not answer all the questions.
We will assess all the answers that you submit.

This exercise is designed to assess your individual problem-solving and coding skills. Please do not use AI tools or coding assistants (including ChatGPT, Copilot, or similar tools).
You are welcome to use Google and refer to official documentation (e.g., language docs, package references, Stack Overflow for syntax clarification). Looking up function usage or syntax is completely fine. What we want to evaluate is your reasoning and implementation, not memorization.


## System Requirements
### Software Dependencies

This code uses `renv` for dependency management. It should be sufficient to first install `renv` from within an R console:

```r
install.packages("renv")
```

Then to install all dependencies, from an R console in the root where this `README` is located:

```r
renv::restore()
```

This will install the specified versions of dependencies as listed in the `renv.lock` file, placing them in a directory under `renv/library/` -- which will often be loaded by default upon opening the RProject, but can be forced via `source("renv/activate.R")`. The dependency installation process should take less than 10 minutes.

## Running the Code

If you have RStudio installed, you can double-click the `CodingChallenge.Rproj` file to open the project from your file manager. Once the project is open in RStudio, open the `CodingChallenge.Rmd` file. With this file open, you can either interactively run the code by clicking the `Run` button in the toolbar above where the file opened, or compile the document to HTML by clicking the `Knit` button in the toolbar above where the file opened.

## Submission Requirements
### Screen Recording

To help us understand your problem-solving process, please record your screen while completing the challenge. 

Privacy Note: Please **mute or disable** notifications (e.g., Focus Mode or Do Not Disturb) before starting to protect your privacy.

Recording Process: Record the ***entire session***, starting from Question 1, and continue through completion.

Recording Area: Ensure you record the **Entire Screen** (or a large enough area) so that we can see you switch between tabs and software (e.g., RStudio, browser, or terminal).

Mac Users: Press Shift + Command + 5, select the screen area, and click Record.

Windows Users (Windows 11): Open the Snipping Tool, select the Video icon, and click New. Select the screen area and hit Start. Once finished, click the preview window to Save the file to your desktop.

Alternative: You may use any recording tool of your choice. Zoom is a great alternative; simply start a meeting, share your entire desktop, and record the session locally to your computer.

Naming: Rename the video file as FirstName_LastName_Recording.mp4.

### Github (Optional)
You are welcome to create a GitHub repository for this challenge, although it is not required. 
If you choose to do so, please include the repository link in your email reply or within a text file in your final upload.

### Final Submission
Please package your recording, code, and any output files into a single folder, compress it as a .zip file, and upload it to the OneDrive folder below:
Note: Maximum of 3 submissions are allowed, but we will only review the latest version provided.
