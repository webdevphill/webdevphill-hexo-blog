---
title: Images To JSON
comments: true
date: 2020-10-28 18:22:51
tags:
- JSON
- Angular
- SASS
- Images
categories: Tools
---

## Create a JSON file for your images

[ImagesToJSON](https://imagestojson.aux.codes) is a website for creating/updating JSON files for a selection of images.

- Generate a JSON file for images 
- Create custom fields
- Reference values from other fields via their 'Id'
- Add to an existing JSON file
- Generates custom fields from existing JSON 
- Save fields interface to your JSON

![](/assets/images/2020-10-28/SelectedFiles.png "ImagesToJson Website")

## Why does it exist?

Images To JSON was developed out of a want to archive a [website](https://rca.aux.codes) that had been moved to the Shopify platform.

The original site was built with a headless CMS backend and not wanting to pay hosting fees to preserve the website, all the backend data had to be converted to JSON files.

This was fine for the majority of the text based data as it could be exported from the CMS, however, the data related to images could not and had to be reconstructed.

## Requirements

The images JSON file needed to contain all the same data as before:
- some of that data could be gathered from the file information
- some of the known required fields data could be derived from file information
- some fields had not yet been determined, thus needed the ability to create new fields and update the JSON file

Input requirements:
- somewhere to add image files
- somewhere to update/create fields for data
- someway to open previously generated JSON files

Output requirements:
- somewhere to view the generated JSON data
- someway to export the JSON file
- someway to save the field settings for later use

## Implementation

The project was developed in Angular and is hosted on Netlify.
The site was divided up into three logical sections:
- image import
- field editing
- JSON output/import

### Image Import

Image import supports 'drag and drop' and file brows.
- once imported, files can be selected and deselected
- if the file name is cropped, mouse hover displays the full file name
- if a JSON file is imported, the image import section is populated with dummy images

![](/assets/images/2020-10-28/ImagesSection.png)

### Field Editing

The fields section has three groups:
- standard file fields
- commonly used/required fields
- custom creatable fields

Each field can be toggled to appear in the JSON file.
Fields can contain content from other fields using their $id.

The field settings are stored in local storage for later use or can be exported in the JSON file.

![](/assets/images/2020-10-28/FieldsSection.png)

### JSON Output

JSON Output is divided into two areas:
- JSON editor for editing the generated JSON
- JSON viewer to get a better idea of what the generated JSON contains

JSON files can be saved and opened from this section.
JSON for the fields settings is also available in this section and saving of these settings can be toggled on/off.

![](/assets/images/2020-10-28/JsonSection.png)

### Conclusion

The app took way to long to implement, the JSON file could have been created manually in the time that it took to create.

However, generating the JSON programmatically ensured their were no typos and new fields could be added as needed in seconds instead of manually adding them to all the images.

And [Rise Community Art](https://rca.aux.codes) has been preserved for future generations. 





