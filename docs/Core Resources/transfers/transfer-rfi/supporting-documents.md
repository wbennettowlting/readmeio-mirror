---
title: Supporting Documents
hidden: true
metadata:
  robots: index
x-privacy:
  view: anyone_with_link
---
> 📘 Why Supporting Documents Are Needed?
>
> When transferring funds to certain countries, local regulations may require additional documentation to verify the purpose or legitimacy of the transaction. To comply with these regulations, Harbor allows you to attach a supporting document directly when initiating a transfer.
>
> Uploading a supporting document can significantly reduce the chances of the transaction being flagged for a Request for Information (RFI), allowing your transfer to proceed more smoothly and with fewer compliance interruptions.

### Field: destination.supporting\_document

The `supporting_document` field accepts a `Base64-encoded` string representing the file content. This field should be included in the destination object when making a `POST /transfers` request.

#### Supported File Types

You may upload the following file formats:

* .png
* .jpg, .jpeg
* .pdf
* .csv
* .docx
* .xlsx

#### File Size Limit

The Base64-encoded file must represent a document smaller than **10MB** before encoding.

<br />

### Example Request Body

```
{
  "destination": {
    ...,
    "supporting_document": "<Base64 Encoded File>",
  },
  ...
}

```
