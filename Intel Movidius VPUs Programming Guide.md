# Intel® Movidius™ VPUs Programming Guide for use with the Intel® Distribution of OpenVINO™ toolkit

## See Also:

ANDREY: Let's link to all 5 of the other HDDL-R docs here, as a list;

- Intel® Movidius™ VPUs Setup Guide for use with the Intel® Distribution of OpenVINO™
- Intel® Vision Accelerator Design with Intel® Movidius™ VPUs HAL Configuration Guide
- Intel® Vision Accelerator Design with Intel® Movidius™ VPUs Workload Distribution User Guide
- Intel® Vision Accelerator Design with Intel® Movidius™ VPUs Scheduler User Guide
- Intel® Vision Accelerator Design with Intel® Movidius™ VPUs Errata

The following section provides information on how to distribute a model across all 8 VPUs to maximize performance.

## Programming a C++ Application for the Accelerator

**Declare a Structure to Track Requests**

The structure should hold:
1.	A pointer to an inference request.
2.	An ID to keep track of the request.<br>

    `struct Request {`<br>
        `InferenceEngine::InferRequest::Ptr inferRequest;`<br>
        `int frameidx;`<br>
    `};`

**Declare a Vector of Requests:**

    `// numRequests` is the number of frames (max size, equal to the number of VPUs in use).
    `vector<Request> request(numRequests);` 

Declare and initialize 2 mutex variables:
1.	For each request.
2.	For when all 8 requests are done.

**Declare a Conditional Variable:** (indicates when at most 8 requests are done at a time)

For inference requests, use the asynchronous IE API calls:<br>

    `// initialize infer request pointer` – Consult IE API for more detail.
    `request[i].inferRequest = executable_network.CreateInferRequestPtr();`

    `// Run inference`<br>
    `request[i].inferRequest->StartAsync();`


**Create a Lambda Function:** (for the parsing and display of results)<br>
Inside the Lambda body use the completion callback function:<br>

    `request[i].inferRequest->SetCompletionCallback`<br>
    `(nferenceEngine::IInferRequest::Ptr context)`


## Additional Resources

- [Intel Distribution of OpenVINO Toolkit home page](https://software.intel.com/en-us/openvino-toolkit)

- [Intel Distribution of OpenVINO Toolkit documentation](https://docs.openvinotoolkit.org)
