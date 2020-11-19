# gcloud-tutorial
An attempt to do some fancy stuff with Google cloud continous integration

# Create the Google Cloud Build functions
First, create a project on Google cloud platform. Name it GCloud tutorial.

![New project](img/new_project.png)

Then enable the Cloud Build API

![Cloud Build API](img/cloud_build_api.png)

Goto the Cloud Build tab and select triggers. First, connect your repository on github. Don't create a push trigger here and select **Skip for now** on step 4.

![Connect Repository](img/connect_repository.png)

Then click on **Create Trigger**, give it a name, select *Push new tag*, connect your repository, when the tag edit box is selected various choices will show and specify that your build recipe will be specified in a cloudbuild.yaml file.
![Create Trigger](img/create_trigger.png)

## The cloud build recipe
Now create a cloudbuild.yaml file with the following content:
```yaml
steps:
- name: 'sglahn/platformio-core:latest'  
  dir: .
  args: ['--version']
```
When a tag is pushed to the github repository this file will be used. The first argument *name* specifies the image to use, which in this case is a public platformio docker image. A full path may be used to fetch an image from any repository, but in this case the latest platformio-core image will be fetched from Docker Hub.

Make a git commit and push.

You may check the History tab under Cloud Build, it should be empty as no tags are yet created.

Now create a git tag v0.0.1, commit and push. ``git push --tags``

Oops, seems like I forgot to add the *cloudbuild.yaml* file!

![Build failed](img/build_failed.png)

Add and commit the *cloudbuild.yaml* file, then make a second tag v0.0.2 and push it to the repo.

Aargh, I forgot to commit cloudbuild.yaml file before I made the tag. A third tag is needed. Now head over to Google Cloud and check the build history:

![Build history](img/build_history.png)

Finally a successful build! Click on the build number to see the details:
![Build log](img/build_log.png)

Lines 1 to 36 are just the setup of the docker image, then on line 37 the output from the ``--version`` argument is shown. Then the build process finish up on lines 38-39.
