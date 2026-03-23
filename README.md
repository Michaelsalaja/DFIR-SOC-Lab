# Digital Forensic Incident Response & Evidence Collection and Analysis

## Scope

This report details a forensic analysis of an image file, including hash generation, case setup in Autopsy, hash verification, and the discovery and recovery of hidden image files. A pre-created forensic challenge image file that was obtained from Techshied’s environment after an incident was used to create a new autopsy case. The goal is to ensure all investigative actions are performed on a preserved copy of the data.

## Verification of Forensic Image Test File

### Step 1: MD5 Hash Creation and Storage

To maintain the integrity of the evidence, an MD5 hash of the original image file created, which creates a unique digital fingerprint that will be used to verify that the file is unmodified or hasn't been altered during the forensic process. It is just simply a fingerprint of the given input.

### Process:

A command-line tool like ***md5sum (Linux/macOS) echo "$(date) $(md5sum 8-jpeg-search.dd)" > hash_verification.txt*** is used. The generated hash value is then saved to a separate text file with the command-line ***cat hash_verification.txt***, which serves as a record of the original state of the evidence.

![image alt](https://github.com/Michaelsalaja/DFIR-SOC-Lab/blob/dcace82a62477bc5ba8a2ab2bd55feae856f8c7b/Screenshot%202026-03-13%20at%2023.37.51.png)

**Step 2:** Prior to setting up the autopsy case, the command ***sudo autopsy*** was used to be directed to <u>http://localhost:9999/autopsy</u>

![image alt](https://github.com/Michaelsalaja/DFIR-SOC-Lab/blob/dcace82a62477bc5ba8a2ab2bd55feae856f8c7b/Screenshot%202026-03-13%20at%2023.38.54.png)

## Autopsy Case Setup & Hash Verification

Autopsy is a powerful open-source digital forensics platform or a forensic browser used to analyze disk images and it enabled Michael Cyber Defense us to look at the content of the drive image, so within the case setup, the hash of the evidence file is verified to ensure that the file being analyzed is the same as the original, unaltered evidence.

**Step 3:** A new autopsy case was created, and the image file is added as a data source. During the data source addition process, Autopsy automatically calculates the MD5 hash of the added file. The calculated hash is, then, stored within the case database.

![image alt](https://github.com/Michaelsalaja/DFIR-SOC-Lab/blob/dcace82a62477bc5ba8a2ab2bd55feae856f8c7b/Screenshot%202026-03-13%20at%2023.42.25.png)

## Task 3: Verifying the hash

The figure below shows the MD5sum of the image and the hash calculated by Autopsy is then compared against the original hash value stored in the in the image file by manually comparing this hash against a known good fingerprint with the one in the image file. A perfect match confirms the integrity of the evidence. That’s not a single file in the image, but that’s the MD5sum for the entire image itself

![image alt](https://github.com/Michaelsalaja/DFIR-SOC-Lab/blob/dcace82a62477bc5ba8a2ab2bd55feae856f8c7b/Screenshot%202026-03-13%20at%2023.44.22.png)

From there, I am led to my main case management screen. My TechShield autopsy case.

![image alt](https://github.com/Michaelsalaja/DFIR-SOC-Lab/blob/dcace82a62477bc5ba8a2ab2bd55feae856f8c7b/Screenshot%202026-03-13%20at%2023.48.05.png)

# Task 4: File Analysis & Hidden Image Recovery

The last part of this section comprises of forensic discovery and Hidden image recovery. This is where File Analysis is conducted to discover and recover artifacts not immediately visible, such as hidden or deleted files. In this case, the objective is to find 5 hidden JPG images. The File Analysis gives me a list of files that are inside this image

## Process:

### File Type identification

*   The search box was used to search for files because autopsy can identify files based on their digital signatures and it enables us to identify the timestamp of files written, access, changed, and created including their sizes. That can be broken down in three parts: The written time, which is sometimes called the M-Time, the access time, which is sometimes called the A-Time, and creation time, which is called the C-Time.

![image alt](https://github.com/Michaelsalaja/DFIR-SOC-Lab/blob/dcace82a62477bc5ba8a2ab2bd55feae856f8c7b/Screenshot%202026-03-13%20at%2023.50.03.png)

*   Navigating to 14 File types > By MIME Type > image/jpeg revealed all files including those that had their extensions changed to conceal them.
*   Before deciding to extract the contents from each file, I used the "Generate MD5 list of files" to button to have the calculated MD5sum values for each file in the current directory

![image alt](https://github.com/Michaelsalaja/DFIR-SOC-Lab/blob/dcace82a62477bc5ba8a2ab2bd55feae856f8c7b/Screenshot%202026-03-13%20at%2023.50.42.png)

*   Recovery: Using autopsy, the contents within each file are then extracted and the 6 discovered JPEG images are exported from the autopsy to a designated folder for further review.

![image alt](https://github.com/Michaelsalaja/DFIR-SOC-Lab/blob/dcace82a62477bc5ba8a2ab2bd55feae856f8c7b/Screenshot%202026-03-13%20at%2023.53.41.png)

![Thumbnail for C:/alloc/file1.jpg showing an orange flower shape with the text "I AM PICTURE #1"](image)
![Thumbnail for C:/alloc/file2.dat showing a red square with the text "I AM PICTURE #2"](image)
![Thumbnail for C:/del1/file6.jpg showing a dark purple square with the text "I AM PICTURE #3"](image)
![Thumbnail for C:/del2/file7.hmm showing a grey square with the text "I AM PICTURE #4"](image)
![Thumbnail showing a teal gradient square with the text "I AM Picture #5"](image)
![Thumbnail for C:/misc/file13.dll:here showing a green puzzle piece shape with the text "I AM PICTURE #10"](image)

## Task 5: Significance of Findings

The discovery and recovery of hidden images are significant from a forensic perspective for several reasons:

*   **Intent to Conceal:** The act of hiding files, either by changing their extension or deleting them to leave them in unallocated space, strongly suggests a deliberate attempt to conceal information. This can indicate malicous acts (guilty mind) on the part of the suspect.
*   **Key Evidence:** The recovered images themselves can contain crucial evidence. They may be contraband, documents relevant to the case, or show a timeline of events. For example, the metadata (EXIF data) within the images could reveal the date, time, and even the geolocation where the picture was taken, providing a timeline and location for a specific event.
*   **Attacker Behavior:** The methods used to hide the files (e.g., specific file names or directory structures) can reveal the suspect's level of technical sophistication and common operating procedures, which can be valuable for future investigations.
*   **Link to Other Artifacts:** The recovered files can be linked to other artifacts in the case, such as web browser history showing where they were downloaded from or chat logs discussing their existence, helping to build a more complete picture of the events.
