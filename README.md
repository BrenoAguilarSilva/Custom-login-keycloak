# Keycloak Custom Login 
<p align="center">
    <img width="400" alt="github-readme-terminal" src="https://pt.quarkus.io/assets/images/brand/quarkus_logo_vertical_450px_default.png">
    <br><br>
    <b>This project uses Quarkus, the Supersonic Subatomic Java Framework.</b>
    <h4 align="center">
        <span>·</span><a href="https://quarkus.io/guides/"> Documentação </a>
    </h4>
</p>

## 📘 Description

This project uses Quarkus, the Supersonic Subatomic Java Framework.

If you want to learn more about Quarkus, please visit its website: https://quarkus.io/ .

In this project we will upload a keycloak container, configure it and add a customized login page

## Running the application in dev mode

You can run your application in dev mode that enables live coding using:
```shell script
./mvnw quarkus:dev
```

> **_NOTE:_**  Quarkus now ships with a Dev UI, which is available in dev mode only at http://localhost:8080/q/dev/.

## Packaging and running the application

The application can be packaged using:
```shell script
./mvnw package
```
It produces the `quarkus-run.jar` file in the `target/quarkus-app/` directory.
Be aware that it’s not an _über-jar_ as the dependencies are copied into the `target/quarkus-app/lib/` directory.

The application is now runnable using `java -jar target/quarkus-app/quarkus-run.jar`.

If you want to build an _über-jar_, execute the following command:
```shell script
./mvnw package -Dquarkus.package.jar.type=uber-jar
```

The application, packaged as an _über-jar_, is now runnable using `java -jar target/*-runner.jar`.

## Creating a native executable

You can create a native executable using: 
```shell script
./mvnw package -Dnative
```

Or, if you don't have GraalVM installed, you can run the native executable build in a container using: 
```shell script
./mvnw package -Dnative -Dquarkus.native.container-build=true
```

You can then execute your native executable with: `./target/login-custom-1.0.0-SNAPSHOT-runner`

If you want to learn more about building native executables, please consult https://quarkus.io/guides/maven-tooling.

## Related Guides

- Keycloak Authorization ([guide](https://quarkus.io/guides/security-keycloak-authorization)): Policy enforcer using Keycloak-managed permissions to control access to protected resources
- OpenID Connect ([guide](https://quarkus.io/guides/security-openid-connect)): Verify Bearer access tokens and authenticate users with Authorization Code Flow

## To clone repository
Clone the project repository to your local environment:
```shell script
git clone https://github.com/BrenoAguilarSilva/Custom-login-keycloak.git
cd Custom-login-keycloak
```

## Upload Containers with Docker-Compose
In the project directory, upload the containers using Docker-Compose:
```shell script
docker-compose up -d
```
This will start Keycloak and the Postgres database.

## Access Keycloak
Access Keycloak through your browser using the URL provided by Docker. You will be redirected to the default Keycloak login page. Use the credentials specified in the docker-compose.yaml file.

## Create a New Realm
After logging in, create a new Realm:
 - No canto superior esquerdo, clique na aba que mostra o nome do Realm atual e selecione "Create realm".
 - Fill in the necessary fields and save.

General Settings:
 - Client type: OpenID Connect
> **_NOTE:_**  Why? OpenID Connect allows clients to verify end-user identity based on authentication performed by an authorization server.
Client ID: <Name of the Provider you want>
 - Client ID: <Name of the Provider you want>
> **_NOTE:_** The Client ID will be used to reference your Client in future accesses.
 - Name: <Name you want>
 - Description: <Optional>
 - Always display in UI: Off

Click “Next” after filling in the fields.

Capability Config:
 - Client authentication: ON
> **_NOTE:_** Sets the access type to confidential when on, and public when off. 
 - Authorization: OFF
 - Authentication flow:
   - Select the options:
     - Standard flow
     - Direct access grants
     - Service account roles

Click “Next” after filling in the fields.

Login Settings:
 - Root URL: Leave blank
 - Home URL: /realms/<YOUR_REALM>/account/
 - Valid redirect URIs:
   - Add the following URLs:
     - /dashboard
     - /* 
     - https://oauth.pstmn.io/v1/callback
 - Valid post logout redirect URIs: Add "+"
 - Web origins: /*

Click “Save” to finish creating the Client.

## Generate Credentials for the Client
 - In the left side menu, go to "Clients" and select the Client you created.
 - Go to the "Credentials" tab and click "Regenerate" under "Client Secret" to generate a new Client Secret.

## Customize the Keycloak Login Page
Download and Configure Keycloak Locally
 - Download the local version of Keycloak from Keycloak Downloads.
 - Unzip the .zip file to a directory of your choice.
 - Unzip Existing Theme
   - Go to the lib → lib → main folder in the unzipped directory.
   - Find and unzip the org.keycloak.keycloak-themes-24.0.4.jar file.
 - Customize the Theme
   - Open the unzipped folder using Visual Studio Code.
   - Inside the home folder, locate and edit the login.ftl file.
   - Create a new folder for your custom theme by copying the keycloak folder and renaming it.
   - Customize the CSS and JavaScript within login → resources → css → login.css.

## Deploy the Custom Theme on Keycloak
Prepare the Theme for Deploy
 - Copy the custom theme folder to a directory called import in the project root.
 - In the terminal, turn the theme folder into a .jar file:
```shell script
jar cf keywind-theme.jar -C keywind .
```
 - Configure Docker-Compose
   - Add the custom theme volume to the docker-compose.yaml file:
```shell script
- ./import/keywind.jar:/opt/keycloak/providers/keywind.jar
```

Restart Containers
 - Restart the containers to apply the new volume:
```shell script
docker-compose down
docker-compose up -d
```

## Apply the Theme in Keycloak
 - Access the Keycloak interface and log in.
 - In the side menu, go to "Realm Settings" → "Themes".
 - In the login option, select your custom theme.

Your custom login theme is now applied in Keycloak.

