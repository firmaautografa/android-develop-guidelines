# 1. Develop & Publish guidelines

## 1.1 Feature Branch Workflow

New features in projects and modules must be integrated following the [Feature Branch Workflow](https://www.atlassian.com/git/tutorials/comparing-workflows/feature-branch-workflow).

Branches develop and master must always contain stable commits and can not have specific test Activities or configurations used for unit testing.

## 1.2 Branch naming

### 1.2.1 Feature branches

Feature branches must contain prefix `feature/` followed by a __lowercase_underscore__ name related to the new functionality to be added; example: `feature/camerax`, `feature/new_enroll_process`.

### 1.2.2 Hotfix branches

Hotfix branches must contain prefix `hotfix/` followed by a __lowercase_underscore__ name related to the bug to be fixed; example: `hotfix/camerax_crash_android_11`, `hotfix/enroll_v2.0`.

# 2. Code review

Before publishing changes in our feature or hotfix branches we must be sure that our code complies with the principles of quality and use of good practices.

## 2.1 Comments

We must implement comments in our code in order to maintain a clear understanding of the logic that we are implementing, especially for the cases in the names of variables or methods that are not completely descriptive.

Example:

```java
public saveAndPost() {
    // encrypt -> send -> update & delete -> return
    apiService.getKeys()
        // encrypt
        .map(resume -> {
            // setting latitude & longitude
            resume.setLatitude(latitude);
            resume.setLongitude(longitude);

            // decode requisition keys
            String key = StringUtils.decodeString(resume.getKey());
            String iv = StringUtils.decodeString(resume.getVector());

            // encrypt data & checksum
            byte[] validatonData = encryptValidatonData(resume, key, iv);
            String checksum = CryptManager.checksumEncrypted(validatonData, key, iv);
            return new EncryptedData(resume.getRequisitionId(), validatonData, checksum);
        })
        // send
        .switchMap(encryptedData -> {
            // posting data to server
            RequestBody requestBody = RequestBody.create(MediaType.parse("application/octet-stream"), encryptedData.data);
            return apiService.saveValidationData(encryptedData.requisitionId, encryptedData.checksum, requestBody);
        })
    }
```

Commented code in our source files are only valid in development stage, before publishing any changes to our repositories we must delete these lines.

## 2.2 SonarLint

Install the SonarLint plugin from AndroidStudio plugins section, once installed we must verify that our code must not contain any major or critical issues.

## 2.2 Format and optimization

While editing an existing or new file (.java, .xml, etc) we must ensure that our changes in code are fully formatted and optimized using either our IDE tools or another plugin or tool.

When using Android Studio we must use the __Optimize imports__ and __Reformat Code__ option, both under the __Code__ menu option.

# License

```
Copyright 2021 FAD Ltd.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```
