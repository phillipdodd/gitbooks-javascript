# Two-Factor Authentication

## What is a Factor?

A **Factor** is a "method or mechanism used to identify someone". There are **three main factors** to implement into an authentication scheme.

### Something You Know

...and _only_ you know. The most common example is the **password.**

### Something You Have

A Bade, ID Card, or Token. 

### Something You Are

Biometrics, using biological attributes of a person to identify them. 

## Time-based One Time Password

This method begins with a **shared secret between the user and server.** This **shared secret** is stored on a **physical device** that the user has in their possession.

This **shared secret** is used to generate a non-repeating, rapidly changing number based on the secret and the time. 

This **generated number** is then input into the website along with a password. To validate, the server uses the **same secret** to generate the number that would be valid at the current time for the user.

More notes at [https://app.pluralsight.com/course-player?clipId=2a888faa-06d6-44e0-9504-5c9441884e79](https://app.pluralsight.com/course-player?clipId=2a888faa-06d6-44e0-9504-5c9441884e79) for putting it into practice

