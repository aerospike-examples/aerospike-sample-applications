## Overview

The purpose of this sample application is to show that Aerospike data structures on top of a key-value store are an effective way to write applications with Aerospike as the only database. To demonstrate, this sample describes the design and implementation of a twitter-like application. The code is easy to follow and substantial enough to be a foundation in learning how to leverage Aerospike's technology and it can also be used as a "seed" application that you can expand.

## Prerequisites: 

- **Aerospike Server** - If you don't have it installed, [click here](http://www.aerospike.com/docs/operations/install) to install it

### AND

- **C# Developers - Aerospike C# Client** — if you don’t have it installed, [click here](http://www.aerospike.com/docs/client/csharp/install) to install it.

### OR

- **Java Developers - Aerospike Java Client** — if you don’t have it installed, [click here](http://www.aerospike.com/docs/client/java/install) to install it.

## You Will Learn

By the end of this, you will have **hands-on experience** in learning:

- Key-Value Operations
- Secondary Indexes
- Equality and Range Filter Queries
- User Defined Functions (Record and Stream UDFs)
- Aggregations

## Application Features

- Write User and Tweet Records
- Retrieve User and Tweet Records
- Update User Record
- Query User and Tweet Records
- Aggregate User Stats by Region

**Note**: To keep this sample application simple and within scope, features such as tracking `following` and `followers` and their tweets have been omitted from the tutorial.

Now let's review information that you would typically want to retrieve from a twitter-like application. This is important because it helps in understanding **how to best structure and store the data.** Effectively leading us to our data models. Consider these:

- User record
- Tweets for a User
- Tweets for all Users
- Users with Tweet count between x and y
- Users with Tweet count between x and y aggregated by their region

Let's map these to Aerospike operations and see how we may retrieve the  information:

- Retrieve User record
    - **Get** operation
- Retrieve Tweets for a User
    - **Batch** read operation *OR* Query using **Equality filter** on Username
- Retrieve Tweets for all Users
    - **Scan All** operation
- Retrieve Users with Tweetcount between x and y
    - **Secondary Index** on Tweetcount
    - Query using **Range filter** on Tweetcount
- Retrieve Users with Tweetcount between x and y; Then aggregate Users by Region
    - Secondary Index on Tweet count
    - Query using **Range filter** on Tweet count
    - **Stream UDF** to aggregate based on Users' region

## Data Models

Data modeling is key to having a well performing application with data models at the core. Data models not only define how the data is structured and stored but, more often than not, they also tend to drive the UI and UX of the application. So, before diving into code, let's review them.

### User

- This is where User Profile is stored
- There will be one record per User
- Key: `username`

Name | Description | Sample Data
--- | --- | ---
username | string | dash
password | string (For simplicity we will store password in plain-text) | asrocks!
gender | string (Valid values are 'm' or 'f') | m
region | string (Valid values are: 'n' (North), 's' (South), 'e' (East), 'w' (West) -- to keep data entry to minimal we will store just the first letter) | w
lasttweeted* | Integer (Stores epoch timestamp of the last/most recent tweet) -- Default to 0 | 1408574221
tweetcount** | Integer (Stores total number of tweets for the user) -- Default to 0 | 32
interests | Array of strings | photography,hiking,travel,house music

\* *lasttweeted* - Storing this attribute will allow us to create a secondary index and then run **time-based queries** such as, top n tweets within last n mins.

\*\* *tweetcount* - This attribute will allow us to create a secondary index and **aggregate users** based on how many times they have tweeted. For example, breakdown of users that have tweeted between min..max times by region. 

### Tweet

- This is where Tweets are stored
- There will be one record per Tweet
- Key: `username:counter`

Name | Description | Sample Data
--- | --- | ---
tweet | string | Put. A. Bird. On. It.
ts | Integer (Stores epoch timestamp of the tweet) | 1408574221
username* | string | dash

\* *username* - Storing this attribute will allow us to create a secondary index and then run queries such as, retrieve all tweets for a given user.

# Next Step

Browse and/or download complete solution in [C#](https://github.com/aerospike/aerospike-sample-applications/tree/master/csharp/tweetaspike) or [Java](https://github.com/aerospike/aerospike-sample-applications/tree/master/java/tweetaspike)

