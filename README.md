# Vimeo-API-resumable-file-uploads
Python implementation using TUS for video uploads in Vimeo.

**Context:**

You’re developing software that uploads videos to Vimeo and you need to programmatically monitor upload progress and confirm the completeness of each upload.

**Story:**
As a developer, I need to upload videos to Vimeo using the resumable approach to ensure smooth and reliable transfers, as well as to efficiently monitor the upload progress and verify the completeness of each upload. 

**UML Sequence Diagram:**

![vimeo_TUS](https://github.com/josev2046/Vimeo-API-resumable-file-uploads/assets/15835851/400d530b-b986-458f-b135-a8e18f8fa3fe)


**Breakdown:**

Initiating the video upload process on Vimeo via the TUS protocol commences with an authenticated POST request to the /me/videos endpoint of the Vimeo API. This request mandates specific headers such as Authorization, Content-Type, and Accept. Additionally, the request body includes parameters like upload.approach set to tus and upload.size indicating the video file's byte length. A successful execution yields a HTTP 200 status code along with crucial video details, notably the upload.upload_link, serving as the upload destination.

Upon video creation, the actual file upload occurs in chunks via the PATCH method to the URL obtained from upload.upload_link. Custom TUS headers like Tus-Resumable, Upload-Offset, and Content-Type accompany the request. Binary data transmission occurs in chunks to optimize transfer efficiency, with the API responding by indicating upload progress via the Upload-Offset header.
For optimal performance, it's recommended to consider the chunk size when using a TUS library. Smaller chunk sizes may require less memory but could result in slower uploads due to more frequent HTTP requests and additional network latency. A chunk size in the range of 128–512 MB is suggested for general-purpose resumable uploads. 

As we upload the video file from a local storage source, the client reads the file and transmits it to Vimeo's servers in chunks using the TUS protocol. These chunks are conveyed as PATCH requests, with the server tracking progress via the Upload-Offset header. This method facilitates upload resumption in case of interruptions or network issues, particularly advantageous for sizable files or unstable network connections.

Finally, to validate upload completion, a HEAD request is dispatched to the upload.upload_link, incorporating headers specifying Tus-Resumable and Accept. The API responds with a HTTP 200 status code and headers revealing Upload-Length and Upload-Offset values. Comparison of these values determines whether the upload is complete: equality signifies successful receipt of the entire video file, while a larger Upload-Length than Upload-Offset denotes an ongoing upload.


[![DOI](https://zenodo.org/badge/793978582.svg)](https://doi.org/10.5281/zenodo.15034988)


