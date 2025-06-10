# Project Setup and Basic Layout Guide

This guide will walk you through setting up your development environment, creating the basic Next.js project, and implementing the fundamental layout for your YouTube clone.

---

## Chapter 1: Project Setup

The goal of this chapter is to configure your development environment and create the basic Next.js project.

### 1.1. Choose Runtime and Package Manager

The course highly recommends **Bun** as both the JavaScript runtime and package manager.

* **Reasons for Bun**:
    * Ensures you have the **same environment as the instructor** for consistency.
    * Can easily run scripts with **TypeScript and ES6 imports** without extra configuration.
    * Seamlessly handles **React 19 peer dependency issues**, avoiding common errors or warnings from npm/yarn.

* **Installation**:
    * **Linux and macOS**: Run `curl -fsSL https://bun.sh/install.sh | bash`.
    * **Windows**: Open PowerShell and run `powershell -c "irm https://bun.sh/install.ps1 | iex"`. (Alternatively, you can install via npm: `npm install -g bun`).
* **Verification**:
    * Confirm Bun installation: Run `bun --version`.
    * It's also recommended to have Node.js (version **18.18 or later**) and npm installed, as Next.js has a Node.js requirement. Verify with `node --version` and `npm --version`.

### 1.2. Create the Next.js Project

You will create a Next.js project named "Carbon" using a specific version for tutorial longevity.

* Run the command:
* For 15.1.2
    ```bash
    bunx create-next-app@15.1.2 carbon
    ```
* For latest
    ```bash
    bunx create-next-app@latest carbon
    ```
    This command uses `bunx` (Bun's equivalent of `npx`) and specifies Next.js version `15.1.2` to ensure stability against future breaking changes.

* When prompted, make the following configuration choices:
    * `Would you like to use TypeScript?` **Yes**
    * `Would you like to use ESLint?` **Yes**
    * `Would you like to use Tailwind CSS?` **Yes**
    * `Would you like to use `src/` directory?` **Yes** (Crucial for organizing files)
    * `Would you like to use App Router (recommended)?` **Yes** (Absolutely crucial for leveraging server components)
    * `Would you like to customize the default import alias (@/*)?` **No** (Defaults to the `@` sign, which is preferred)
    * `Would you like to use TurboPack for `bun dev`?` **No** (To ensure consistency with the tutorial environment)

* Once the project is created, navigate into its directory: `cd carbon`.
* Open the project in your IDE. **VS Code** is highly recommended for consistent extensions.

### 1.3. Project Structure Overview

The core logic of your application will primarily reside within the **`src` folder**. This folder will contain:

* **`app`**: For routing.
* **`modules`**: For organizing features and components.
* **`lib`**: For utility functions and configurations.
* **`scripts`**: For backend scripts.

### 1.4. Integrate Shadcn UI

Shadcn UI is a collection of reusable components that you can copy and paste into your app, allowing you to build a custom component library.

* **Installation**:
    ```bash
    bunx shadcn-ui@2.1.8 init
    ```
* For latest  
    ```bash
    bunx shadcn-ui@latest init
    ```
    The specific version `2.1.8` is used for tutorial consistency. Bun avoids issues with React 19's peer dependencies during this process, unlike npm which might throw errors.

* **Configuration Choices** during `shadcn-ui init`:
    * Style: **New York**
    * Base Color: **Neutral**
    * Use CSS variables: **Yes**

* **Update Tailwind CSS Configuration**: After Shadcn UI installation, you will need to update your `tailwind.config.ts` file to include the `modules` folder so Tailwind CSS styles are properly applied to components within it.

* **Install All Components**:
    ```bash
    bunx shadcn@2.1.8 add --all
    ```
* For latest
    ```bash
    bunx shadcn@latest add --all
    ```
    This command installs all Shadcn UI components at once, simplifying future usage. Components will be imported using aliases like `components/UI/button`, which default to the `src` folder, streamlining imports. The source code of these components is directly available, allowing for easy modification.

### 1.5. VS Code Extensions

* Install the **Tailwind CSS Intellisense** extension. This extension provides auto-completion, hover-over CSS definitions, and typo detection, greatly enhancing your Tailwind CSS development experience.

### 1.6. Initial Verification

* Run the development server: `bun run dev`.
* Visit `http://localhost:3000` in your browser. You should see the basic Next.js project running.
* To test Tailwind CSS, modify a component (e.g., `src/app/page.tsx`) and add classes like `font-bold` or `text-red-500` to an element. You should see the styles applied.

---

## Chapter 2: Basic Layout

This chapter focuses on building the foundational layout of your application, including the sidebar and navbar, and understanding Next.js App Router fundamentals.

### 2.1. Adding the Logo Asset

* **Download Logo**: Obtain the `logo.svg` asset (which resembles the YouTube logo but is distinct) from the provided Guist or the course's public folder.
* **Place in Project**: Drag and drop the downloaded `logo.svg` file into your project's **`public` folder**.
* **Implement in Page**:
    * Open `src/app/page.tsx`.
    * Import `Image` from `next/image`.
    * Use `<Image src="/logo.svg" alt="logo" width={50} height={50} />` to display the logo. The `Image` component requires `width` and `height` for optimization.

### 2.2. Changing the Project Font

* Modify the **root layout** file: `src/app/layout.tsx`.
* Import the `Inter` font from `next/font/google`:
    ```typescript
    import { Inter } from 'next/font/google';

    const inter = Inter({ subsets: ['latin'] });
    ```
* Apply the `inter.className` to the `<body>` tag:
    ```typescript
    <body className={inter.className}>
        {children}
    </body>
    ```
* You can now apply Tailwind CSS classes like `text-extra-large`, `font-semibold`, and `tracking-tight` to text elements for a bolder, tighter look.

### 2.3. Understanding Basic App Router Folders

The Next.js App Router uses a file-system based routing system.

* **Creating Routes**: A folder inside the `app` directory with a `page.tsx` file inside will define a route.
    * Example: `src/app/feed/page.tsx` creates the `/feed` route.
* **Dynamic Routes**: Use square brackets `[folderName]` to create dynamic segments in a route.
    * Example: `src/app/feed/[videoID]/page.tsx` will match `/feed/123`, `/feed/abc`, etc.
    * To access dynamic parameters in Next.js 15, you must **`await`** them within an `async` server component:
        ```typescript
        async function Page({ params }: { params: { videoID: string } }) {
            const videoID = await params.videoID;
            // ... use videoID
        }
        ```
        This is crucial because dynamic APIs (like `params`, `searchParams`, `cookies`, `draftMode`, `headers`) are now asynchronous in Next.js 15 server components.
* **Route Groups**: Folders enclosed in **parentheses `(folderName)`** within the `app` directory are considered "route groups".
    * They **do not affect the URL path**.
    * They are used for **organizing routes** into logical groups (e.g., `src/app/(home)/page.tsx` still renders at `/`).
* **Layouts (`layout.tsx`)**: This is a reserved file name used to define reusable UI that is shared across multiple routes in a folder or route group.
    * A layout component **must accept a `children` prop** (of type `React.ReactNode`) and render it.
    * Example: A `layout.tsx` in `src/app/(home)/` would apply to all routes within the `(home)` group.
    * The **root layout (`src/app/layout.tsx`)** is unique as it defines the `<html>` and `<body>` tags and **should not be moved**.

### 2.4. Implement Module-Based File Structure

Adopt a modular approach to organize your application's code.

* Create a **`src/modules` folder**.
* Inside `modules`, you will create feature-specific folders (e.g., `home`, `studio`, `videos`).
* Within these module folders, organize your components, hooks, API procedures, and utilities (e.g., `src/modules/home/UI/layouts/home-layout.tsx`).
* The **`app` folder should strictly contain routing files** (`page.tsx`, `layout.tsx`, `route.ts`) and minimal UI logic.

### 2.5. Update Tailwind CSS for New Modules

* Edit `tailwind.config.ts`.
* Ensure that your `content` array includes the `modules` folder so that Tailwind CSS can scan and apply styles to your new modular components.

### 2.6. Develop the Home Layout

* Create a reusable home layout component: `src/modules/home/UI/layouts/home-layout.tsx`.
* Encapsulate your main content with the **`SidebarProvider`** component from Shadcn UI.
* Apply a `padding-top` to your content container (e.g., `padding-top-16` if your navbar has a height of `h-16`) to prevent it from being hidden behind the fixed navbar.

### 2.7. Develop the Home Navbar

* Create the navbar component: `src/modules/home/UI/components/home-navbar/index.tsx`.
* Use a `<nav>` HTML element with Tailwind classes for **fixed positioning**, `top-0`, `left-0`, `right-0`, and `h-16`.
* Implement **Flexbox** (`flex`, `items-center`, `justify-between`) for proper alignment of elements within the navbar.
* Include the **`SidebarTrigger`** component from Shadcn UI to toggle the sidebar.
* Add the `next/image` logo and a placeholder for a search bar and a sign-in button.

### 2.8. Develop the Home Sidebar

* Create the sidebar component: `src/modules/home/UI/components/home-sidebar/index.tsx`.
* Utilize **`Sidebar` and `SidebarContent`** components from Shadcn UI to build the sidebar structure.
* Organize navigation links into `MainSection` (e.g., Home, Subscriptions, Trending) and `PersonalSection` (e.g., History, Liked Videos, Playlists). These sections will use Shadcn UI components like `SidebarGroup`, `SidebarMenuItem`, `SidebarMenuButton`, and `SidebarMenuContent`.
* **Client Components for Interactivity**: For features like highlighting the active navigation item using `usePathname` from `next/navigation`, ensure your sidebar components are marked with `"use client"` at the top of the file.
* **Customize Icon**: In `src/components/UI/sidebar.tsx`, change the default `panelLeft` icon to `menuIcon` for the sidebar trigger button.
* **Enable Collapsible Mode**: Add the `collapsible` prop to the `Sidebar` component to allow collapsing (e.g., `collapsible='icon'`).
* **Optional: Remove Animations**: If desired, you can remove `transition-all` and `duration` Tailwind classes from `src/components/UI/sidebar.tsx` to remove the default opening/closing animations.

### 2.9. Set up Global `Bun Run Dev All` Command (for future chapters)

While not strictly used in Chapter 2, it's beneficial to set this up now for upcoming chapters that involve webhooks.

* Install `concurrently` as a development dependency: `bun add -D concurrently@9.1.2`.
* Modify your `package.json` scripts to run both your development server and your local tunnel (Angrok, covered in Chapter 5) concurrently:
    ```json
    {
      "scripts": {
        "dev": "next dev",
        "dev-web-hook": "bunx --bun angrok http --url [https://your-static-domain.ngrok-free.app](https://your-static-domain.ngrok-free.app) --port 3000",
        "dev-all": "concurrently \"bun run dev-web-hook\" \"bun run dev\""
      }
    }
    ```
    (Replace `https://your-static-domain.ngrok-free.app` with your actual static domain obtained later.)
* From now on, you will use **`bun run dev-all`** to start your development environment.
