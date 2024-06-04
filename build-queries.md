# Build queries for user data

## Goal

The goal of this lesson is get comfortable with the [Prisma Client API](https://www.prisma.io/docs/reference/api-reference/prisma-client-reference) and explore some of the available database queries you can send with it. You'll learn about CRUD queries, relation queries (like nested writes), filtering and pagination. Along the way, you will run another migration to introduce a second model with a [*relation](https://www.prisma.io/docs/concepts/components/prisma-schema/relations)* to the `User` model that you created before.

## Setup

You will continue working in the project you worked on in lesson one. Make sure your development server is up and running. If it isn’t run the following command within your project to start it up:

`npm run dev`

## Hints

- ***Type yourself*, don't copy and paste**
    
    To learn and really *understand* what you are doing for each task, be sure to **not copy and paste the solution** but type out the solution yourself (even if you have to look it up). 
    
- **Use the autocompletion**
    
    Prisma Client provides a number of queries that you can send to your database using its API. You can learn about these queries in the [documentation](https://www.prisma.io/docs/reference/tools-and-interfaces/prisma-client) or explore the API right in your editor using *autocompletion*.
    
    To invoke the autocompletion, you can open `src/index.ts` and type following *inside* of the `main` function (you can delete the comment `// ... your Prisma Client queries will go here` that's currently there):
    
    ```tsx
    import { PrismaClient } from '@prisma/client'
    
    const prisma = new PrismaClient()
    
    async function main() {
      const result = await **prisma.** // autocompletion will show up if you type this
    }
    
    main()
      .catch((e) => console.error(e))
      .finally(async () => await prisma.$disconnect())
    ```
    
    - Expand for a screenshot of the autocompletion
        
        ![images/Untitled.png](./images/Untitled.png)
        
    
    Once you typed the line `const result = await prisma.` ****into your editor, a little popup will be shown that lets you select the options for composing a query (e.g.  selecting a *model* you want to query or using another top-level function like `$queryRaw` or `$connect`). Autocompletion is available for the *entire* query, including any arguments that you might want to provide!
    
- **Visualize data in Prisma Studio**

## Tasks

Your tasks will be split into two sections: *authentication* and *user management.* 
These sections have to do with the functionality you are completing in the application.

## Authentication

The functions for this set of tasks can be found in `app/utils/auth.server.ts`

### **Task 1: Complete the `register` function**

Currently, the signup feature of your site will not actually create a user. It will instead act as if a user was properly created and direct you to the login screen.

The `register` function is being passed an object that contains the registration form’s data. Query for a  *unique* user who’s email matches the email provided in the form. 

The result of that query should be stored in the `exists`  constant inside of that function.

- Solution
    
    Import `prisma` from `app/utils/prisma.server.ts`
    
    ```tsx
    import { prisma } from "~/utils/prisma.server";
    ```
    
    Then add this query:
    
    ```tsx
    export async function register(user: RegisterForm) {
    	const exists = await prisma.user.count({ 
    		where: { email: user.email }
    	});
    	// ...
    }
    ```
    

### **Task 2: Complete the `login` function**

Your login function is not very secure at the moment.  So long as the user provides a valid email and a password, they will be successfully sent to the login screen. 

Replace the static `user` data with a query for a *unique* user whose email matches the email provided in the login form. Only retrieve the fields in your document that you need in the function.

- Solution
    
    ```tsx
    const user = await prisma.user.findUnique({
      select: { id: true, password: true },
      where: { email }
    })
    ```
    

## User Management

The functions for this set of tasks can be found in `app/utils/user.server.ts`

### **Task 1: Complete the `createUser` function**

Replace the `// create user` comment with a query that stores a new user using the `user` parameter in the function and the hashed password. 

Return the new user’s `id` and `email` from the function in an object. 

*This is used in the `register` function.*

- Solution
    
    Import `prisma` from `app/utils/prisma.server.ts`
    
    ```tsx
    import { prisma } from "~/utils/prisma.server";
    ```
    
    The resulting function should look like this:
    
    ```tsx
    export const createUser = async (user: RegisterForm) => {
      const passwordHash = await bcrypt.hash(user.password, 10);
    
      const newUser = await prisma.user.create({
        data: {
          email: user.email,
          password: passwordHash,
          profile: {
            firstName: user.firstName,
            lastName: user.lastName
          }
        }
      })
    
      return {
        id: newUser.id,
        email: newUser.email
      };
    };
    ```
    

### **Task 2: Complete the `getOtherUsers` function**

Return an array of every user **except** for the user who’s `id` matches the `userId` parameter’s value. Select these fields: `id`, `email`, `profile` and order the results by the user’s first name in ascending order.

*This is used to fetch the users to display on the home page’s user list.*

- Solution
    
    ```tsx
    export const getOtherUsers = async (userId: string) => {
      return await prisma.user.findMany({
        select: {
          id: true,
          email: true,
          profile: true
        },
        where: {
          id: {
            not: userId
          }
        },
        orderBy: {
          profile: {
            firstName: 'asc'
          }
        }
      })
    };
    ```
    

### **Task 3: Complete the `getUserById` function**

Query for a single user whose `id` matches a `userId` provided to the function. Only retrieve the `id`, `email`, and `profile` fields.

*This is used throughout the application to retrieve a user’s data.*

- Solution
    
    ```tsx
    export const getUserById = async (userId: string) => {
      return await prisma.user.findUnique({
        select: {
          id: true,
          email: true,
          profile: true,
        },
        where: {
          id: userId,
        },
      });
    };
    ```
    

### **Task 4: Complete the `updateUser` function**

Prisma Client generates an awesome set of types that you have access to and can use throughout your application to ensure your data and the queries you generate are in a shape that makes sense with your data.

Import and use the `Profile` type generated by Prisma as the type for this function’s `profile` parameter. Then write an update query that uses that data to update a user’s `profile` embedded document whose `id` matches the `userId` provided in the parameters.

*This function is used in the profile settings modal*

- Solution
    
    Import the `Profile` type:
    
    ```tsx
    import { Profile } from "@prisma/client";
    ```
    
    Write the query:
    
    ```tsx
    export const updateUser = async (userId: string, profile: Profile) => {
      await prisma.user.update({
        where: { id: userId },
        data: {
          profile: {
            update: profile,
          },
        },
      });
    };
    ```
    

### **Task 5: Complete the `deleteUser` function**

The last function relating to your user data will delete a user whose `id` matches the `id` parameter passed to the function.

- Solution
    
    ```tsx
    export const deleteUser = async (id: string) => {
      await prisma.user.delete({ where: { id }})
    };
    ```
    

Play around in your application a bit! You can:

- Register for an account
- Log in to your account
- Update your profile settings
- View the list of other users

> You may need to clear your cookies if you had attempted to log in before this lesson.
>