
# S2I runtime image for OpenJDK11 minimized  

## Getting started  

### Files and Directories  
| File                   | Required? | Description                                                  |
|------------------------|-----------|--------------------------------------------------------------|
| Dockerfile             | Yes       | Defines the base builder image                               |
| s2i/bin/assemble       | Yes       | Script that builds the application                           |
| s2i/bin/usage          | No        | Script that prints the usage of the builder                  |
| s2i/bin/run            | Yes       | Script that runs the application                             |
| test/run               | No        | Test script for the builder image                            |
| test/test-app          | Yes       | Test application source code                                 |

#### Dockerfile
Minimizes the OpenJDK11 runtime and copies it into a new debian-slim image

#### S2I scripts

##### assemble
Does nothing, but required by the S2I process

##### run
*Run's* the spring-boot application deployed 

##### usage (optional) 
Create a *usage* script that will print out instructions on how to use the image.

#### Create the builder image
The following command will create a builder image named openjdk-11-mini based on the Dockerfile that was created previously.
```
docker build -t openjdk-11-mini .
```
The builder image can also be created by using the *make* command since a *Makefile* is included.

Once the image has finished building, the command *s2i usage openjdk-11-mini* will print out the help info that was defined in the *usage* script.

#### Testing the builder image
The builder image can be tested using the following commands:
```
docker build -t openjdk-11-mini-candidate .
IMAGE_NAME=openjdk-11-mini-candidate test/run
```
The builder image can also be tested by using the *make test* command since a *Makefile* is included.

#### Creating the application image
The application image combines the builder image with your applications source code, which is served using whatever application is installed via the *Dockerfile*, compiled using the *assemble* script, and run using the *run* script.
The following command will create the application image:
```
s2i build test/test-app openjdk-11-mini openjdk-11-mini-app
---> Building and installing application from source...
```
Using the logic defined in the *assemble* script, s2i will now create an application image using the builder image as a base and including the source code from the test/test-app directory. 

#### Running the application image
Running the application image is as simple as invoking the docker run command:
```
docker run -d -p 8080:8080 openjdk-11-mini-app
```
The application, which consists of a simple static web page, should now be accessible at  [http://localhost:8080](http://localhost:8080).
