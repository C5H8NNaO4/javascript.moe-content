
## 📖 Setting Up Strapi Backend with TypeScript and Localization

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

*GET http://localhost:1337/api/articles*


This will return all the articles stored in your Strapi backend, including their localized content.

---

### 🚀 Step 7: Fetch Articles from Strapi in Next.js

Now that our Strapi backend is set up and articles are created, let’s modify the Next.js frontend to fetch this data. Here's the code to fetch articles with Static Site Generation (SSG) from the Strapi API:

```js
// app/[locale]/page.tsx
async function getArticles() {
  const res = await fetch('http://localhost:1337/api/articles', {
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

"*/src/api/article/routes*"

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
  "collectionName": "blog_posts",
  "pluginOptions": { "i18n": { "localized": true } },
  "info": {
    "singularName": "blog-post",
    "pluralName": "blog-posts",
    "displayName": "Blog Post"
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
    "category": {
      "type": "relation",
      "relation": "manyToOne",
      "target": "api::category.category",
      "inversedBy": "blog_posts"
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
    },
    "tags": {
      "type": "relation",
      "relation": "manyToMany",
      "target": "api::tag.tag",
      "inversedBy": "blog_posts",
      "pluginOptions": { "i18n": { "localized": false } }
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
- **category**: A many-to-one relation field linking to a `category` content type.
- **publishedAt**: A datetime field for the publication date.
- **metaTitle**: A string for SEO meta titles, localized.
- **metaDescription**: A text field for SEO meta descriptions, localized.
- **tags**: A many-to-many relation linking to `tag` content type, not localized.

#### Enabling Localization (i18n):
To enable localization for your content type, we need to:
1. Enable the **i18n plugin** for the entire content type, as done above with `"pluginOptions": { "i18n": { "localized": true } }`.
2. Enable localization for each field that needs to be localized (e.g., title, content, metaDescription).

With the schema above, **title**, **content**, **excerpt**, **metaTitle**, and **metaDescription** will all be localized per language, whereas **slug**, **coverImage**, **category**, and **tags** will not be localized.

---

### ⚙️ Step 8.2: Creating the Router, Controller, and Service Files

Once you've defined the content type, you’ll also need to set up the **router**, **controller**, and **service** files to handle the routing, API logic, and business logic for the content.

#### Router:
Create the following file:

```ts
// src/api/article/routes/article.ts
import { factories } from '@strapi/strapi';

export default factories.createCoreRouter('api::blog-post.blog-post');
```

#### Controller:
Create the following file:

```ts
// src/api/article/controllers/article.ts
import { factories } from '@strapi/strapi';

export default factories.createCoreController('api::blog-post.blog-post');
```

#### Service:
Create the following file:

```ts
// src/api/article/services/article.ts
import { factories } from '@strapi/strapi';

export default factories.createCoreService('api::blog-post.blog-post');
```

---

Now, your content type, API routes, controller, and services are all set up!

In the next step, you can run Strapi locally and fetch your content using the API.

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE1ODUzMzg4MThdfQ==
-->