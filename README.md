# FASTAPI
FastAPI boilerplate

## Setup

1. Create a virtual environment.
 ```sh
    python3 -m venv .venv
 ```
2. Activate virtual environment.
```sh
    source /path/to/venv/bin/activate`
```
3. Install project dependencies `pip install -r requirements.txt`
4. Create a .env file by copying the .env.sample file
`cp .env.sample .env`

5. Start server.
 ```sh
 python main.py
```

**API DESIGN DOCUMENTATION**
The API follows OpenAPI 3.0 specifications, which provide a standardized way to describe the endpoints, parameters, and responses.

**API LICENSE**
Apache 2.0

**DESCRIPTION**
 The api supports the functionalities for:

-- Authentication and Registration
-- Messaging 
-- Payments (Flutterwave)
-- User & Organization management
-- Superadmin interface
-- User settings and profile management(edit user details)
-- Page for GPDR cookies management and consent
-- Notifications
-- Blog management
-- Viewing chart data

**API OVERVIEW**
  AUTHENTICATION
  The API uses Bearer Token Authentication. You need to include the JWT token in the Authorization header for protected endpoints:
      `Authorization: Bearer <your_token_here>`  
    AUTH ENDPOINTS:
         **Method	Endpoint	      Description**
            POST  	/signup	       Create a new user 
            POST	  /login    	    Log in a user
            POST	  /logout	       Log out the authenticated user (PROTECTED)
            POST	  /magic-link	   Generate a magic link
            
   USER MANAGEMENT ENDPOINT:
            Method       	Endpoint	             Description
             GET	       /superadmin/users	      Get a list of users (PROTECTED)
             GET	       /users/{user_id}	       Get user details (PROTECTED)
             PATCH	     /users/{user_id}	       Edit user details (PROTECTED)
             
   ORGANISATION MANAGEMENT ENDPOINT:
             Method	     Endpoint               	        Description
             GET	       /superadmin/organisations  	     Get a list of organisations (PROTECTED)
             GET	      /organisations/{organisation_id}	  Get organisation details (PROTECTED) only `authenticated users` and `superadmin`

   MESSAGING:
             POST /email: Send newsletters.
             POST /invite: Generate and send an invite link via email.
    
   PAYMENTS:
            POST /payments: Process a payment using Flutterwave. (PROTECTED)

   SUPERADMIN INTERFACE:
           GET /superadmin/users/{user_id}/export: Export user data (superadmin only).
           GET /superadmin/organisations: Get a list of organisations (superadmin only).
           GET /superadmin/payments: Get a list of all payments (superadmin only).
           GET /superadmin/activity-log: Get the activity log (superadmin only).
           GET /superadmin/payments/{user_id}: Get a list of all payments for a specific user (superadmin only).
           GET /superadmin/activity-log{user_id}: Get the activity logs for a specific user (superadmin only).

  GDPR Cookies Management:
           GET /cookies: Get information about cookies used.
           POST /consent: User consent for cookies.
           
  NOTIFICATIONS:
           GET /notifications: View all user notifications.
           GET /notifications/{notification_id}: Get a specific notification.
           
  BLOG MANAGEMENT:
           GET /blogs: Get a list of blogs.
           GET /blogs/category/{blog_id}: Get blog details.
           PATCH /blogs/edit-content/category/{blog_id}: Edit blog content (admin/editor/superadmin only).

  VIEWING CHART DATA:
           GET /charts: Get chart data. (PROTECTED), (superadmin and analyst only)

BELOW IS THE LINK TO VIEW THE HTML OF THE API DESIGN:
` https://app.swaggerhub.com/apis-docs/FAVOURADEBOSE_1/Auth/1.0.0 `


**DATABASE SCHEMA DESIGN DOCUMENTATION**
The database contains the necessary entities required to store information from the system and below is the schema definition for all
entities and the relationships among them.

TABLES WITH THEIR RESPECTIVE ATTRIBUTES
   USERS:
      id integer [primary key]
      email varchar [unique]
      password varchar
      name varchar
      subscribe_to_newsletter boolean
      created_at timestamp
   
   ROLES:
      id integer [primary key]
      name varchar [unique]
      description varchar
      permissions json
      user_roles
      user_id integer
      role_id integer
   
   BLOGS:
      id integer [primary key]
      title varchar
      content text [note: 'Content of the blog post']
      author_id integer
      created_at timestamp
   
   ORGANISATIONS:
      id integer [primary key]
      name varchar [unique]
      description varchar
      user_organisations
      user_id integer
      organisation_id integer
   
   PAYMENTS:
      id integer [primary key]
      amount decimal
      currency varchar
      method varchar [note: 'Payment method, e.g., flutterwave']
      user_id integer
      created_at timestamp
   
   BLACKLISTED_TOKENS:
      id integer [primary key]
      token varchar [unique]
      user_id integer
      created_at timestamp
   
   USER_LOGS:
      id integer [primary key]
      user_id integer
      action varchar
      created_at timestamp


RELATIONSHIPS BETWEEN TABLES:
    -- Each blog post is written by a user (one user can write many blog posts).
    -- Each user can have multiple roles assigned to them (one user can belong to many roles).
    -- Each role can be assigned to many users.
    -- Each payment is made by a user (one user can make many payments).
    -- Each user can belong to multiple organizations (one user can be part of many organizations).
    -- Each organization can have many users associated with it.
    -- Each blacklisted token is linked to a specific user (one user can have multiple blacklisted tokens).
    -- Each user log entry is associated with a specific user (one user can have many log entries).

**BELOW IS THE LINK TO VIEW THE DIAGRAM FOR THE ENTITY RELATIONSHIP DIAGRAM**
  ` https://drive.google.com/file/d/132sT_Z7_cK5EAa8gClT4BPUI0R64yQiy/view?usp=sharing `


