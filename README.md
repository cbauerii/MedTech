# MedTech

## Table of Contents
1. [Overview](#Overview)
1. [Product Spec](#Product-Spec)
1. [Wireframes](#Wireframes)
2. [Schema](#Schema)

## Overview
### Description
The purpose of our app is to help educate users on how to properly handle a medical emergency if / when one occurs. It will contain information about various emergencies, prevention tips, how to spot early symptoms etc. Furthermore, it will focus on how to react to a current emergency, guiding a user through the necessary steps. For example, after confirming that an emergency is a heart attack, the application would provide step by step instructions on what to do. It will also include a button to quickly call authorities and will contain any helpful tips, videos, gifs, music, etc. to best guide the user.

### App Evaluation
- **Category:** Medical / Health
- **Mobile:** Given that not everyone carries their computer on them at all times, having MedTech on mobile makes it more flexible for paramedics, nurses, doctors, and other hospital workers to help individuals during a medical crisis.
- **Story:** Analyzes user’s health conditions and medical info, provides tutorials on major medical emergencies, provides prevention tips
- **Market:** Everyone! Anyone willing to help in the case of a medical emergency, people with pre-existing medical conditions, hospital workers
- **Habit:** This application will be accessed by the user when there is a medical emergency, when medical info needs to be updated, and maybe during medical appointments
- **Scope:** We aim to have at least multiple tutorials to educate users on what to do in a medical emergency. We also aim to parse the medical data into a card that can be handed out to paramedics and doctors with the consent from the user.

## Product Spec

### 1. User Stories (Required and Optional)

**Required Must-have Stories**

* User can sign-up
* User can log-in / log-out
* User can take a quiz
* User can update medical information

**Optional Nice-to-have Stories**

* HIPAA Consent
* Terms and Conditions
* Daily Check-In

### 2. Screen Archetypes

* Registration
   * Username / Password
   * Medical History Quiz
* Log-In
   * Username / Password
* Home Screen
   * Main Hub of Navigation basically
* Personal Health Tracking
   * Diet, weight, exercise, etc
* Medical Info & Prevention Tips
   * List of medical emergencies
   * Relevant medical information
* Current Emergency Help
   * Instructions / Tutorial
   * HIPAA / Consent Form (to allow hospitals to gain MedTech card)

### 3. Navigation

**Tab Navigation** (Tab to Screen)

* Personal Health Tracking
* Medical Info & Prevention Tips
* Current Emergency Help

**Flow Navigation** (Screen to Screen)

* Forced Log-In
* Shortcut -> Current Emergency Help
* Current Emergency Help -> Quiz

## Wireframes
<img src="https://cdn.discordapp.com/attachments/950515792825434134/950541110365798430/20220307_184931.jpg" width=300>

<img src="https://cdn.discordapp.com/attachments/950515792825434134/950545301234659338/IMG_2093.jpg" width=300>

### [BONUS] Digital Wireframes & Mockups
![](https://i.imgur.com/NtJ4TGJ.png)

### [BONUS] Interactive Prototype

## Schema 
### Models

**Model: User**
| Property | Type | Description |
| -------- | -------- | -------- |
| username | String | unique id for user login |
| password | String | unique password id for user login |
| dateOfBirth | String | used to verify consent or something idk, also to calculate age |
| legal_name | String | FirstName LastName |
| consent | Boolean | consent to release information to hospitals, no third-parties involved, HIPAA contract |
| createdAt | DateTime | date when user created account |

**Personal Information**
| Property | Type | Description |
| -------- | -------- | -------- |
| main_address | Dictionary | current place of residence of user |
| emergency_contacts | Dictionary | optional list of emergency contacts, defined by user |
| updated_info | DateTime | date when personal information was last updated (default field) |

**Medical Information**
| Property | Type | Description |
| -------- | -------- | -------- |
| height | Double | height of user |
| weight | Double | weight of user |
| family_history | Dictionary | medical history in user's family |
| current_medications | Dictionary | list of current meds taken by user |
| allergies | Dictionary | current relevant allergies of user |
| past_surgeries | Dictionary | lists past surgeries of user |
| updated_info | DateTime | date when medical information was last updated (default field) |

**Medical Emergency**
| Property | Type | Description |
| -------- | -------- | -------- |
| emergencyName | String | name of the emergency |
| emergencySteps | Dictionary | steps to take to escalate the emergency |
| emergencyStepsVisual | Dictionary | visual steps to take to escalate the emergency |
| currentLocation | String | used to identify nearest hospital maybe |
| consentToEmergencyContacts | Boolean | consent given by user to notify contacts |
| consentToNearestHospital | Boolean | consent given by user to notify nearby hospital |
| consentToParamedics | Boolean | consent given by user to notify paramedics |
| updated_info | DateTime | date when medical emergency information was last updated (default field) |

**List of Medical Emergencies we will address:**
* Heart attack
* Choking
* Seizure
* Drowning
* Stroke
* Anaphylactic Shock
* Severe injuries
* Poison Control
* Overdosing (alcohol and / or drugs)
* Breathing problems

**Extra Medical Emergencies**
* Severe dehydration
* Hypothermia
* Heat stroke
* Loss of consciousness
* Mental Health related things
* Severe burns
* Concussion
* Paralysis
* Broken bones
* Electrocution
* Chemical burns
* Asthma
* Diabetic reactions


### Networking
#### list of network requests by screen

* Profile Screen
    * (Read/GET) Query logged in user object
    * (Update/PUT) Update user profile image

* Home Profile Screen
    * (Read/GET) Query all existing user information or notes
    * (Create/POST) Add new info or notes
    * (Delete) Delete existing info or notes
    * The following code snip will be edited and used to account for user info/notes
         ```swift
         let query = PFQuery(className:"Post")
         query.whereKey("author", equalTo: currentUser)
         query.order(byDescending: "createdAt")
         query.findObjectsInBackground { (posts: [PFObject]?, error: Error?) in
            if let error = error { 
               print(error.localizedDescription)
            } else if let posts = posts {
               print("Successfully retrieved \(posts.count) posts.")
           // TODO: Do something with posts...
            }
         }
         ```
         
         
#### [OPTIONAL: List endpoints if using existing API]
 ##### An API of ICD (International Classification of Diseases)
- Base URL - https://icd.who.int/icd/api

   HTTP Verb | Endpoint | Description
   ----------|----------|------------
    `GET`    | /icd/release/11/{linearizationname}   | returns basic info on the linearization such as MMS together w/ the list of available releases
    `GET`    | /icd/release/11/{releaseId}/{linearizationname}/{id} | get a linearization entity
    `GET`    | /icd/entity/{id} | returns basic info on specific latest release of ICD-11 Foundation entity
    `GET`    | /icd/release/10   | lists the available ICD-10 releases
