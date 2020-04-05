# My Shared Air

## Short pitch

With My Shared Air, you can keep track of your own interactions and help stop the virus from spreading - without ever compromising our personal integrity. It uses bluetooth technology in order to track your interactions without ever recording personal information about you, your device, or the person you met. It does all this while allowing only certified people in the healthcare system to report confirmed cases - giving you an instant notification if you or someone you have met gets sick.

## Problem Description 

Currently, there is a very hard trade-off between restricting the spread of the virus and restricting the freedom of the public. This is due to two factors: 

1. We do not know where the virus is and therefore need to put everyone in lockdown, despite most people being healthy and at low risk. 
2. We can do contact tracing to find out who is at risk. But most solutions jeopardize your data privacy, and risks granting governments powers they can misuse. 


## Solution Description

By doing privacy-preserving (or distributed anonymized) contact tracing we solve both of the above problems at once: 
1. By tracing the interactions between people we can track down the spread of the virus, and encourage those who are at risk of having been infected to take appropriate measures and go get tested. 
2. By storing people’s data locally on their phones, and only transmitting non-personal information we can preserve their privacy and ensure that there isn’t even the possibility of misuse. No trust needed. 

We call this solution **My Shared Air**.

The flow is as follows: 
1. When running in the background the app looks for other devices with My Shared Air installed. 
2. When someone is encountered you exchange a randomly generated string of letters - long enough to be unique to your interaction, as well as the distance and duration of the encounter. All of which are stored locally on your phone. 
3. When you get tested and have notified the testing authorities that you are using My Shared Air and your app Id, authorized health personnel use BankId to log into the website and inform you of the test result.
4. If you test positive you get a notification, and the ability to release your random identifiers. This will allow everyone who has interacted with you to see that they have been exposed to risk, without seeing who or where this risk came from. 

Out security practices are: 
1. No identifiable information is sent over the network
2. Your personal information never leaves your device
3. No centralized data storage containing personal data
4. Always open source code, so you can be sure we aren’t hiding something. 

## Action Plan 

We identify the following steps as crucial for making this technology beneficial to society:

**A fully working prototype of the app and backend**
1. Android-specific functionality
2. List the app on the App Store and Google Play. 

**Convince authorities of the importance of data privacy**
1. We can provide the same safety to our people as more authoritarian regimes without becoming more like them. 
2. If the authorities are not on-board with this then the project will stop dead in its tracks. 
3. We want to reach out to: 
Government officials
Politicians 
Key influential figures

**Integration with the SMInet or 1177**
1. In order to not interfere with the testing pipeline, the app should be integrated with the current digital systems for test results.
2. We, therefore, want to (and have already) reach out to Inera to get this integration going. 

**Convince the general public of the value** 
1. This solution is more valuable the more people use it. It is therefore key that the general population see the value in this app and trust it. 
    1. We want to reach out to
    2. News outlets
    3. Influencers
    4. Facebook marketing (the price of ads on fb is minuscule compared to the economic impact)

**Make sure the systems can scale**
1. Firebase provides an excellent way for us to easily scale the usage of this app. 
2. Modifications like grouping the map into coarse regions will allow scaling users into the millions. 

**Consolidate solutions with other solutions from across the EU and world.**
1. Fortunately, other teams around the world have realized the value of privacy preserving contact tracing. 
2. In order to have the best effect possible we need to harmonize across regions. 
3. The app does not need to be the same, but data formats and standards should be enforced to allow co-operation. 

**Additional features to increase value**
1. Integrate with a self-diagnostics chatbot to allow for  “soft” contact tracing 
2. Use GPS in a safe way to trace places of transmission alongside people

## Demo Video
[![MySharedAir](https://youtu.be/Ite7J0jusb8)](https://youtu.be/Ite7J0jusb8)

## Tech Description

**The solution is split into three pieces:** 
1. The app installed on each person’s device
2. The server directing the traffic between devices 
3. The website where test-results can be registered 

**The app:**
* The current version of the app is focused on iOS, and developed in Swift. 
    * Interactions between phones are handled using Bluetooth LE:
    * The phone searches in the background for other devices running the app
    * When a peer is discovered a connection is established. The devices agree on a randomized interaction token.
    * The distance (measured by rssi) is then monitored over the course of the interaction.
    * When the interaction is over the distances and the durations are synthesized into three categories of transmission risk: “low”, “medium”, “high”. 
    * The risk and interaction id are then stored locally on the device. 
    * These are deleted after 14 days.
* When the unit is notified that it has tested positive: 
    * A push notification is sent to the user, such that they can take the right measures
    * All interactions with risk “medium” or “high” from the past 14 days are collected and pushed to the network of other devices. With no other metadata. 
* When the unit receives interaction IDs:
    * It checks if any ID matches an ID in the local database

**The server:** 
* The server is built using Firebase to be able to scale quickly
* Incoming test results are directly routed to the right app. And the app ID is temporarily allowed to upload data to the server. 
* Any incoming interaction IDs trigger the server to push out that data as a message to all devices in the network. 
* The interaction IDs are temporarily stored for up to 24h such that they can reach phones that happen to be offline. 

**The healthcare portal:**
* The portal is built using next.js
* A healthcare worker verifies themselves using BankID. They can then enter the random ID of the person who shall receive the test results. 
* This information is then propagated through the Firebase server to the relevant user. 

## Github repos
* The app: https://github.com/distributed-contact-tracing/contact-tracing-peer
* The server: https://github.com/distributed-contact-tracing/flaming-service
* The portal: https://github.com/distributed-contact-tracing/portal 

## Team description
We are a group of current and former students at KTH specializing in fields such as data science, computer science and UI design.

Oliver is an engineer and a researcher, currently finishing his MSc in Computer Science at NYU. Previously, he worked with data and machine learning at Uber and Spotify. He did his undergraduate in Engineering Physics at KTH, where he also dabbled in entrepreneurship. 

Ulme - a software engineer at Microsoft - also studied engineering physics at KTH and did a MSc in machine learning from KTH with one year at University of Washington. He has a background working on machine learning and AI in the academic world and has co-authored three research papers - most recently published at EMNLP 2019 in Hong Kong. He was part of KTH:s research submission to Amazon Alexa Prize 2018.

Axel studies his second year at KTH while working part-time as an iOS developer at insurtech startup Hedvig. He enjoys coding front-end and creating UI’s, but is academically looking to expand his knowledge in the field of optimization theory. Concurrently, he keeps some personal app projects going.

Viktor is a third-year bachelor student at KTH with a background in Investigation and security operations in the Swedish Armed Forces. He works part-time as a software consultant specializing in React and Node. He enjoys playing music, climbing and runs 10 km in 42 minutes.

