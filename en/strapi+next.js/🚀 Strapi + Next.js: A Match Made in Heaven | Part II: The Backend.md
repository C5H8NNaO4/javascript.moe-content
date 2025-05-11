# Strapi + Next.js: A Match Made in Heaven | Part III: The Backend

This is **Part III** of a series of posts, walking you through creating a Strapi + Next.js project from scratch. [**Part II**](https://javascript.moe/en/blog/strapi-next-js-a-match-made-in-heaven-part-ii-the-frontend-udq30yay3xcml783tybydxpp)

## 🚀Part III: The Backend

In this part of the guide, we’ll walk through the steps to set up Strapi locally, create a content type for articles, enable localization, and expose the necessary APIs. This will allow us to fetch and render content on the Next.js frontend.

### 🛠️ Step 1: Install Strapi with TypeScript

First, we need to create a new Strapi project. For this tutorial, we’ll use Strapi v5 and set it up with TypeScript.

Run the following commands to install Strapi globally and create a new project:

```bash
yarn create strapi-app backend --typescript
```

Once the command finishes, navigate into the `backend` directory:

```bash
cd backend
```

### 🏃‍♀️ Step 2: Start the Strapi App Locally

To run Strapi locally, use the following command:

```bash
yarn develop
```

This will start your Strapi project in development mode. Once it’s up and running, you should see a message indicating that Strapi is running on `http://localhost:1337`.

Go ahead and open this URL in your browser. You’ll be prompted to create an admin user account to access the Strapi admin panel.

---

### 🧩 Step 3: Create the Articles Content Type

Now that we have Strapi up and running, let’s create a content type called "Articles."

1. From the Strapi admin panel, go to the **Content-Type Builder**.
2. Click on **Create new collection type** and name it **Article**.
3. Add the following fields:
   - **Title** (type: Text)
   - **Content** (type: Rich Text)
   - **Published** (type: Boolean)

Once you've added the necessary fields, click **Save** to generate the Article content type.

---

### 🌍 Step 4: Enable Localization for the Articles Content Type

Strapi v5 comes with built-in support for localization. To enable this feature for our articles:

1. In the Strapi admin panel, go to **Settings** > **Internationalization** (i18n).
2. Click on **Enable** to activate the i18n feature.
3. Now, go back to the **Content-Type Builder** and select the **Article** content type.
4. For each field you want to localize, click on the field and enable the localization option by checking **Enable localization**. This will allow content to be stored in different languages.

For the **Title** and **Content** fields, enable localization so you can add content in multiple languages.

Once you’ve enabled localization, you can now create articles in different languages!

---

### 🔧 Step 5: Create Some Article Content

Now that we have our **Article** content type and localization set up, let’s add some sample articles.

1. Go to the **Content Manager** and click on **Article**.
2. Click **Add New Article**.
3. Add an article title and content in your default language (e.g., English). Then, you can add translations for the title and content in the other languages (e.g., French, German) by switching to the different languages at the top of the content entry form.

You should now have multiple versions of your articles, each localized according to the language settings.

---

### 🛠️ Step 6: Expose the Article API

By default, Strapi exposes a RESTful API for each content type. To fetch articles from the frontend, we’ll need to ensure that the appropriate routes are open.

1. Go to **Settings** > **API Tokens** and create a new public token (or use the default `public` token).
2. Now, to fetch articles with their localized content, go to the **Settings** > **Roles & Permissions** and make sure the **Public** role has permission to access the **Article** API.

Once the API is exposed and the permissions are set, you can access your articles via this endpoint:

_GET http://localhost:1337/api/articles_

This will return all the articles stored in your Strapi backend, including their localized content.

---

### 🚀 Step 7: Fetch Articles from Strapi in Next.js

Now that our Strapi backend is set up and articles are created, let’s modify the Next.js frontend to fetch this data. Here's the code to fetch articles with Static Site Generation (SSG) from the Strapi API:

```js
// app/[locale]/page.tsx
async function getArticles() {
  const res = await fetch("http://localhost:1337/api/articles", {
    next: { revalidate: 60 }, // Enable ISR
  });
  const { data } = await res.json();
  return data;
}

export default async function HomePage() {
  const articles = await getArticles();

  return (
    <main>
      <h1>Articles</h1>
      <ul>
        {articles.map((article: any) => (
          <li key={article.id}>{article.title}</li>
        ))}
      </ul>
    </main>
  );
}
```

Now, when you visit the page corresponding to a language (e.g., `localhost:3000/en`, `localhost:3000/fr`), you should see the localized articles being displayed dynamically based on the locale.

---

## ✅ Final Thoughts

By combining **Strapi** with **Next.js** and leveraging the built-in i18n features, you can easily create multi-lingual, content-rich websites with minimal setup. Strapi's flexibility and localization features make it a perfect headless CMS for modern applications, while Next.js' performance optimizations like SSG and ISR ensure your site remains fast and scalable.

### 📝 Step 8: Creating the Article Content Type with Router, Controller, and Service

In Strapi, when you create a new content type (like "Article"), it automatically generates some basic files for you. However, if you want more control over the routing, business logic, and data handling, you can customize Strapi by defining the **router**, **controller**, and **service** manually.

#### 🛠️ What Each File Does

- **Router**: The router is responsible for defining the HTTP endpoints (e.g., `GET`, `POST`, `PUT`, `DELETE`) that clients can interact with. It tells Strapi which controller should handle a particular request.
- **Controller**: The controller contains the logic for handling the incoming HTTP requests. It is where we define what happens when the API receives a request, such as fetching data, processing it, and sending a response.
- **Service**: The service is where we define the business logic for interacting with the database. It abstracts the logic for fetching, creating, updating, or deleting records in Strapi’s database.

#### 🚧 Step 8.1: Create the Article Router

To define the routes for the **Article** content type, you'll create a router. In your Strapi project, navigate to the following directory:

"_/src/api/article/routes_"

## 📝 Step 8: Creating the Article Content Type with Localization (i18n)

In Strapi v5, content types like "Article" can be created easily using the CLI or the content-type builder. However, for more control, we can also define them manually. Below, we walk through the steps to create the **Article** content type, along with the necessary **router**, **controller**, and **service** files, and how to enable localization (i18n).

### ⚙️ Step 8.1: Creating the Article Content Type

To define the **Article** content type with localization, we'll need to manually add the necessary configuration in a JSON file.

#### Create the Content Type

To start, create a JSON file for the **Article** content type in the following path:

```
/src/api/article/content-types/article/schema.json
```

Here is an example schema definition for the **Article** content type, with localization enabled for specific fields:

```json
{
  "kind": "collectionType",
  "collectionName": "articles",
  "pluginOptions": { "i18n": { "localized": true } },
  "info": {
    "singularName": "article",
    "pluralName": "articles",
    "displayName": "Article"
  },
  "options": {
    "draftAndPublish": true
  },
  "attributes": {
    "title": {
      "type": "string",
      "required": true,
      "pluginOptions": { "i18n": { "localized": true } }
    },
    "slug": {
      "type": "uid",
      "targetField": "title"
    },
    "content": {
      "type": "richtext",
      "required": true,
      "pluginOptions": { "i18n": { "localized": true } }
    },
    "excerpt": {
      "type": "richtext",
      "required": true,
      "pluginOptions": { "i18n": { "localized": true } }
    },
    "coverImage": {
      "type": "media",
      "multiple": false,
      "required": true,
      "allowedTypes": ["images"]
    },
    "publishedAt": {
      "type": "datetime"
    },
    "metaTitle": {
      "type": "string",
      "pluginOptions": { "i18n": { "localized": true } }
    },
    "metaDescription": {
      "type": "text",
      "pluginOptions": { "i18n": { "localized": true } }
    }
  }
}
```

#### Explanation of Fields:

- **title**: A required string field that is localized for different languages.
- **slug**: A unique identifier (UID) that is generated based on the `title` field.
- **content**: A rich text field that is required and localized.
- **excerpt**: A short text field used for summaries, also localized.
- **coverImage**: A media field for an image, required, with allowed types restricted to images.
- **publishedAt**: A datetime field for the publication date.
- **metaTitle**: A string for SEO meta titles, localized.
- **metaDescription**: A text field for SEO meta descriptions, localized.

#### Enabling Localization (i18n):

To enable localization for your content type, we need to:

1. Enable the **i18n plugin** for the entire content type, as done above with `"pluginOptions": { "i18n": { "localized": true } }`.
2. Enable localization for each field that needs to be localized (e.g., title, content, metaDescription).

With the schema above, **title**, **content**, **excerpt**, **metaTitle**, and **metaDescription** will all be localized per language, whereas **slug**, **coverImage**, **category**, and **tags** will not be localized.

---

### ⚙️ Step 8.2: Creating the Router, Controller, and Service Files

Once you've defined the content type, you’ll also need to set up the **router**, **controller**, and **service** files to handle the routing, API logic, and business logic for the content.

To make this easier, you can bootstrap these files with the Strapi CLI, but you must replace the contents.

```
# bootstrap the folder structure
npx strapi generate
```

#### Controller:

Create or replace the following file with this content

```ts
// src/api/article/controllers/article.ts
import { factories } from "@strapi/strapi";

export default factories.createCoreController("api::article.article");
```

#### Router:

```ts
// src/api/article/routes/article.ts
import { factories } from "@strapi/strapi";

export default factories.createCoreRouter("api::article.article");
```

#### Service:

```ts
// src/api/article/services/article.ts
import { factories } from "@strapi/strapi";

export default factories.createCoreService("api::article.article");
```

---

Now, your content type, API routes, controller, and services are all set up!

#### TypeScript errors in the IDE

It may be the case that your IDE complains about missing types in one of these files.
To resolve this, you need to build the backend once which will regenerate the correct type definitions.

In the next step, you can run Strapi locally and fetch your content using the API.

## 🌐 Accessing Strapi Admin Panel and Setting Up Permissions

Once you've set up your Strapi backend locally, it's time to visit the **Strapi Admin Panel** to manage your content types, configure permissions, and start testing your API endpoints.

### Step 1: Access the Strapi Admin Panel

After running Strapi with the command:

```
yarn dev
```

Strapi will be available at `http://localhost:1337` by default. To access the admin panel, open your browser and go to:

```
http://localhost:1337/admin
```

If this is your first time running Strapi, you'll be prompted to create an admin account. Fill in your credentials and log in.

### Step 2: Enable Permissions for the Article Content Type

Once logged into the admin panel, you need to configure permissions for your content type (in this case, **Article**), so that users (or API consumers) can access the data.

1. **Navigate to Roles & Permissions**:
   - In the admin panel, go to the **Settings** section (found in the bottom left corner).
   - Under the **Users & Permissions** section, click on **Roles**.
2. **Configure Permissions for Public Role**:
   - In the **Roles** section, select the **Public** role (this is typically used for unauthenticated users).
   - Scroll down to the **Permissions** section and find your **Article** content type.
   - Check the permissions you want to enable. For example, you might want to enable:
     - **Find** – to allow users to retrieve articles.
     - **Find One** – to allow users to view a single article.
3. **Save Changes**:
   After enabling the necessary permissions, make sure to click the **Save** button in the top right corner.

### Step 3: Testing the API Endpoint

With the permissions set, you're now ready to test the API endpoint for your **Article** content type.

1. **Test Fetching Articles** (GET Request):
   Open your browser or an API client like **Postman** or **Insomnia**, and test the following endpoint to retrieve the list of articles:

```
GET http://localhost:1337/api/articles
```

If the request is successful, you'll get a response with the list of articles from your Strapi backend, similar to this:

```json
{
  "data": [],
  "meta": {
    "pagination": { "page": 1, "pageSize": 25, "pageCount": 0, "total": 0 }
  }
}
```

The response doesn't contain any data, because you haven't created any content yet.
Head over to the Content Manager and create a new Article, then hit the API endpoint again.

It should now respond with your articles, similar to this response:

```json
{
  "data": [
    {
      "id": 2,
      "documentId": "kwz45r8ko4huwnl6zz72jfgy",
      "title": "Test",
      "slug": "test",
      "content": "Test",
      "excerpt": "Test",
      "publishedAt": "2025-05-11T16:53:06.457Z",
      "metaTitle": null,
      "metaDescription": null,
      "createdAt": "2025-05-11T16:53:04.655Z",
      "updatedAt": "2025-05-11T16:53:06.441Z",
      "locale": "en"
    }
  ],
  "meta": {
    "pagination": { "page": 1, "pageSize": 25, "pageCount": 1, "total": 1 }
  }
}
```

### ✅ Thoughts

By now, you’ve successfully created a localized `Article` content type in Strapi, set up proper permissions, and confirmed that your API is returning structured data. You’ve got a functional headless CMS running locally, ready to power your Next.js frontend.

With this setup, you’re now equipped to build a fast, localized, and SEO-friendly site backed by structured, dynamic content. You can continue modeling more complex content types, configure media uploads, or integrate GraphQL if needed—all from this solid foundation.

### 🧩 Using Environment Variables to Connect Next.js to Strapi

To keep your setup clean and production-ready, it's best practice to avoid hardcoding your Strapi API URL in your frontend code. Instead, use environment variables to dynamically reference your API host. This also makes it easy to switch between local, staging, and production environments.

#### 1. Create an Environment Variable

In your Next.js project, create a `.env.local` file at the root and add:

```text
STRAPI_API_URL=http://localhost:1337
```

> 💡 You can later replace this value with your deployed Strapi instance URL when going live.

#### 2. Load the Environment Variable in Your App

In your `app/[locale]/page.tsx` file (or wherever you fetch your articles), update your fetch logic to use the environment variable:

```tsx
// app/[locale]/page.tsx
async function getArticles() {
  const baseUrl = process.env.STRAPI_API_URL;
  const res = await fetch(`${baseUrl}/api/articles`, {
    next: { revalidate: 60 }, // Enable ISR
  });
  const { data } = await res.json();
  return data;
}

export default async function HomePage() {
  const articles = await getArticles();

  return (
    <main>
      <h1>Articles</h1>
      <ul>
        {articles.map((article: any) => (
          <li key={article.id}>{article.title}</li>
        ))}
      </ul>
    </main>
  );
}
```

#### 3. Restart the Dev Server

After modifying your `.env.local` file, start the Next.js dev server: `yarn dev`.

You should now see plain text of your articles. The only thing left now is to build a proper
frontend, flesh out the backend and **deploy** your app so it can be reached online.


### What's next

🚀 **Next up:** In the following section, we'll walk you through deploying both your **Strapi backend** and your **Next.js frontend**—so your site can go live with minimal effort and cost, leveraging the platforms **Railway** and **Vercel**.

Stay tuned!
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE4NDMwMTYwMTVdfQ==
-->