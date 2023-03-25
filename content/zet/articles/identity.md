---
title: "Tokens and Identity on the Internet"
date: 2022-12-18T00:55:47+01:00
tags:
- DevOps
- Explanations
- Article
---

# Introduction

Have you ever thought about your identity on the internet? How does LinkedIn know it is you when you log in to LinkedIn? And when you allow LinkedIn to post to your Twitter, how does LinkedIn access your account and not your kindergarten teacher's account?

This is a "Mischa Explains" article where I attempt to explain a concept in my own words as simply as possible. I use the Feynman technique and pretend to explain it to a 12-year-old. 

# Identity

The first step in this process is identity. You need a starting point; for many of us, this can be our Google account. You signed up for this account and probably verified this with your phone number. 

This relates to **authentication**. Authentication is the process of verifying identity. You'll need to provide the correct password when you log in to your Google account. You must give a valid password to log in to your account and access the resources. Google uses your password to **authenticate** that it is you. 

# Authorization

Then we have **authorization**. Authorization means granting access to particular resources. For example, let's say you are working in the science classroom at school. In the classroom is a bookcase that everybody can use: it is not dangerous, and every student can take the books they need without asking for permission. In the back of the science classroom is a cabinet that contains chemicals. It would be very dangerous if everybody could go into the cabinet and take out the sulphuric acid. Not everybody might know how dangerous sulphuric acid is. That's why the cabinet is locked. 

If you need something from the chemicals cabinet, you need to ask permission from the teacher. You need to be **authorized** by the teacher to take out the sulphuric acid. When you make your request, the teacher may ask you questions to ensure you know what you are doing. He might even ask you for your school ID card because he has not seen you before. The teacher **authenticates** you by asking for your school ID, and then he **authorizes** you to take out the sulphuric acid. 

# Tokens

How do we accomplish this on the internet? 

To verify identities on the internet, we have identity providers. Google is an identity provider. Azure AD is also an identity provider. An open-source identity provider is Keycloak. 

Identity providers use *tokens* to verify identity and authorize access to resources. There are two types of tokens: ID tokens and access tokens. And for each token, there is an associated protocol. 

## ID tokens

OpenID Connect, also known as OIDC, is an open standard for authentication. Identity providers have agreed with each other that they will use this standard. When you go through an OpenID workflow, the result is an ID token, proving that the user has been authenticated. 

Your school ID card is the ID token in our science class example. When you started at your school, you went through a registration process. Your parents probably handled this. Your name was written down, and the school verified that it was you by looking at your passport and talking to your parents. The result of this process was your school ID card, which you use to borrow books from the library. The school ID card proves that you are a student of that school and that you can use the facilities at the school.

## Access tokens

These are specifically designed to allow access to a resource. For example, this resource could be a file on a server or a database. 

Access tokens are strictly for authorization and use the OAuth 2.0 standard.

In our science class example, the token would be the key to the chemicals cabinet. The teacher authorizes you to access the cabinet and gives you the key to the cabinet. 

## Putting it Together

Now let's put it together with an example. 

You just created a new Facebook account and want to add all your friends. However, you have a Google account, and Facebook can use the contacts in your Google account to automatically add all of your friends. 

Your Google account can only be accessed by you, and your contacts are locked away behind a password. But it is possible to grant Facebook access to this. 

On Facebook, you select the "import contacts from Google" function. Facebook sends you to Google, and Google will ask you to log in. Google is the teacher in our science class example. Google needs you to **authenticate** to prove that it is you. When this is done, Google generates an ID token using OIDC for Facebook: Google gives Facebook a school ID that it can use. 

Next, Facebook needs access to the contacts in your Google account. In our example, Facebook asks to take the sulphuric acid from the chemicals cabinet. You will see a menu that specifies what Facebook wants to do, and you need to give your permission. When you give your permission, Google generates an OAuth 2.0 token for Facebook. In other words, Google gives the key to the chemicals cabinet to Facebook, and Facebook is now authorized to take the sulphuric acid. 

When both of these tokens are generated, Facebook contacts Google and asks if it can take the sulphuric acid from the chemicals cabinet.

Google, the teacher, asks Facebook for the school ID, and Facebook shows the ID card it received earlier. When Google is satisfied with the ID and successfully authenticates Facebook, it gives Facebook the key to the chemicals cabinet. Facebook is now authorized to take out the sulphuric acid. Facebook is now authorized to access the contacts in your Google account.

# Links

You can use these resources to learn more about this topic:

[An Illustrated Guide to OAuth and OpenID connect](https://www.youtube.com/watch?v=t18YB3xDfXI)

[ID Tokens vs Access Tokens - Do you know the difference?](https://www.youtube.com/watch?v=M4JIvUIE17c)

[Microsoft Learn: ID Tokens](https://learn.microsoft.com/en-us/azure/active-directory/develop/id-tokens)

[Microsoft Learn: Security Tokens](https://learn.microsoft.com/en-us/azure/active-directory/develop/security-tokens)
