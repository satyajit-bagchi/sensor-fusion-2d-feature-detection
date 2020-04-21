# Midterm project

1. Implement a vector for dataBuffer objects
    - ```cpp
        dataBuffer.push_back(frame);
        if (dataBuffer.size() > dataBufferSize) {
            dataBuffer.erase(dataBuffer.begin());
        }
        ```

2. Implement detectors HARRIS, FAST, BRISK, ORB, AKAZE, and SIFT and make them selectable by setting a string accordingly.
    - Implemented an if else taking string detectorType

3. Remove all keypoints outside of a pre-defined rectangle and only use the keypoints within the rectangle for further processing.

    - ```cpp
            for (auto& kp : keypoints) 
            {
                if (vehicleRect.contains(kp.pt)) {
                    keypoints_filtered.push_back(kp);
                }
            }
            keypoints = std::move(keypoints_filtered);```
4. Implement descriptors BRIEF, ORB, FREAK, AKAZE and SIFT and make them selectable by setting a string accordingly.
    - Implemented an if else taking string descriptorType

	
5. Implement FLANN matching as well as k-nearest neighbor selection. Both methods must be selectable using the respective strings in the main function.
    - ```cpp
      matcher = cv::DescriptorMatcher::create(cv::DescriptorMatcher::FLANNBASED);```
6. Use the K-Nearest-Neighbor matching to implement the descriptor distance ratio test, which looks at the ratio of best vs. second-best match to decide whether to keep an associated pair of keypoints.
```cpp
        auto k = 2;
        vector<vector<cv::DMatch>> knn_matches;
        matcher->knnMatch(descSource, descRef, knn_matches, k);

        for (auto it = knn_matches.begin(); it != knn_matches.end(); ++it) {
            auto distance_ratio = (*it)[0].distance / (*it)[1].distance;
            if (distance_ratio > 0.8) {
                matches.push_back((*it)[0]);
            }
        }
```

7. Count the number of keypoints on the preceding vehicle for all 10 images and take note of the distribution of their neighborhood size. Do this for all the detectors you have implemented.
    - Calculated distribution of number of keypoints for each input image from each detector. See excel sheet
8.  Count the number of matched keypoints for all 10 images using all possible combinations of detectors and descriptors. In the matching step, the BF approach is used with the descriptor distance ratio set to 0.8.

    - Calculated number of matched points for all images for each detector + descriptor combination. See excel sheet

- Log the time it takes for keypoint detection and descriptor extraction. The results must be entered into a spreadsheet and based on this data, the TOP3 detector / descriptor combinations must be recommended as the best choice for our purpose of detecting keypoints on vehicles.

    
  - Calculated time to calculate and match keypoints for all images for each detector + descriptor combination. See excel sheet
  - FAST detects a large number of key points ~5000 with a low std. deviation. which is robust
  - All decriptors show similar performance
  - Based on performance top 3:
      - FAST + ORB
      - FAST + BRIEF
      - FAST + FREAK
