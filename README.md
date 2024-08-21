# Image to PDF: Free and Unlimited Image Conversion with Power Automate

This project provides a solution for converting images to PDF format using Microsoft Power Automate, allowing you to perform unlimited conversions effortlessly. Follow the steps below to set up and use this solution:

## How It Works

1. Create a Folder in SharePoint:
- First, create a folder named "image_to_pdf" under the "Documents" section in SharePoint.

2. Create a Power Automate Flow:
- Log in to Power Automate and create a new flow.
   
3. Set Up the Trigger:
- Select the SharePoint trigger "When a file is created (properties only)" and choose the folder path you just created.

4. Get File Content:
- Add the SharePoint action "Get file content" and select the File Identifier (e.g., example_file_id).
  
![image-1](https://github.com/korhanh/PowerAutomate_Image_to_PDF/blob/main/1.png)

5. Convert to Base64:
- Next, add a Compose action to convert the retrieved file to Base64 format. Name this action "base64". Use the following expression:

```plaintext
base64(outputs('Get_file_content')?['body'])
```
![image-2](https://github.com/korhanh/PowerAutomate_Image_to_PDF/blob/main/2.png)

6. File Type Check:
- Add a Condition action and choose "or" to ensure the flow continues only if the file is a .png, .jpg, or .jpeg:
```plaintext
  last(split(triggerOutputs()?['body/{FilenameWithExtension}'], '.'))
```
![image-3](https://github.com/korhanh/PowerAutomate_Image_to_PDF/blob/main/3.png)

7. HTTP Request:
- Add an HTTP action:

  - Method: POST
  - URI: https://zetaleap.com/image/upload_image/
  - Headers: Authorization Bearer (Obtain your activation code from zetaleap.com)
  - Use the following JSON for the body:
```json
{
  "image": "data:image/@{last(split(triggerOutputs()?['body/{FilenameWithExtension}'], '.'))};base64,@{outputs('base64')}"
}
```

**This code helps determine the image type and send the Base64 data. Ensure you name the actions as shown in the visuals to avoid any issues later.**

8. Create the PDF File:
Finally, add the SharePoint "Create file" action to save the newly converted PDF file in the SharePoint folder you created earlier.

File Name:
```plaintext
 body('HTTP')?['file_name']
```
File Content:
```plaintext
 base64ToBinary(body('HTTP')?['file_base64'])
```

![image-4](https://github.com/korhanh/PowerAutomate_Image_to_PDF/blob/main/4.png)

## Features

- Unlimited Conversion: Convert as many images to PDF as needed without any restrictions.
- Easy to Set Up: Follow the step-by-step guide to create the flow and customize it according to your needs.
- Efficient Workflow: Automate the PDF conversion process seamlessly within SharePoint.

## Requirements

- Microsoft Power Automate subscription
- Basic knowledge of SharePoint and Power Automate flow creation

## Getting Started

- Follow the steps outlined above to add the necessary actions to your Power Automate flow.
- Customize the site and folder paths as needed.
- Use the "base64" variable to convert images and create PDFs.
  
![image-5](https://github.com/korhanh/PowerAutomate_Image_to_PDF/blob/main/5.png)

## Contributions and Feedback

Contributions to this project are welcome! If you encounter any issues or have suggestions for improvement, feel free to open an issue or submit a pull request.

## License

This project is licensed under the [MIT License](https://github.com/korhanh/PowerAutomate_Image_to_PDF/blob/main/LICENSE).

Feel free to use, modify, and distribute this project according to the terms specified in the license.

## Credits

This project is created and maintained by [korhanh]([link_to_your_github_profile](https://github.com/korhanh)).

If you use this project or find it helpful, consider giving it a star on GitHub!

