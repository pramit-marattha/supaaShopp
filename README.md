<p align="center">
<img src="https://user-images.githubusercontent.com/37651620/159424112-2faca207-6e1d-42b7-9b2f-b889cb5a693e.png" alt="Supabase Ecommerce logo">
</p>

`SuperbaseEcommerce` is the the app we'll be building on in this tutorial. It is simply an online ecommerce shopping site where users can browse all of the products, bookmark their favorite products, and even purchase the products. It is similar to an Amazon app, but it is simpler because we will not implement any actual payment or shipping procedures. Here's a live demonstration of the final version of the app. This is how your app should look after you finish this tutorial. Feel free to experiment with it to get a sense of all the features we will be implementing.

<p align="center">
<img src="https://user-images.githubusercontent.com/37651620/158058874-6a86646c-c60e-4c39-bc6a-d81974afe635.png" height="250" width="250"alt="Supabase Ecommerce logo">
</p>

![Demo](https://user-images.githubusercontent.com/37651620/159170940-d8c210c4-5b9d-47f7-b5d8-547bea3106fc.png)

![Demo](https://user-images.githubusercontent.com/37651620/159170944-b48ead6f-6482-4234-a5ea-7b20335ca7fe.png)

So, in this tutorial, we'll learn how to build this full-stack app with `Next.js`, the react framework, `NextAuth.js`, for implementing passwordless and OAuth authentication, `Supabase`, for persisting app data into a PostgreSQL database and stashing media files and information, and `Prisma`, for making it simple to read and write data from and to the database from our app.

This article tutorial covers many topics and technical concepts necessary to build a modern full-stack app, even if this app is a simplified version of a more advanced ecommerce site like Amazon. You should be able to use all of the technologies covered in this tutorial, including react, nextjs, prisma, supabase, and others, but most importantly, you should be able to build any full-stack app using those technologies. You'll go at your own speed and intensity, with us guiding you along the way. After completing this guide, the goal of this article is to provide you with the tools and techniques you'll need to build a similar app on your own.To put it another way, this tutorial will not only teach you how to use those technologies in great detail, but it will also provide you with the proper mixture of principles and application to help you grasp all of the key concepts so that you can proudly build your own apps from scratch later part on this article.

Let's start with the react portion and build our application. The first step is to install Node.js if it isn't already on your computer. So, go to the official Node.js website and download the most recent version. Node js is required to use the node package manager, abbreviated as npm. Now launch your preferred code editor and navigate to the folder. For this article tutorial, we'll be using the VScode code editor.

### Setting up SupabaseEcommerce project.

There is a [Github repository](https://github.com/pramit-marattha/SupabaseEcommerce) dedicated to this project, which consists of three branches. Clone the [`SupabaseEcommerce-starter`](https://github.com/pramit-marattha/SupabaseEcommerce/tree/SupabaseEcommerce-starter) branch to get started.

![Github](https://user-images.githubusercontent.com/37651620/158067824-de726446-b049-4ebb-9c2f-adbecd571d64.png)

The `Main` branch contains the entire `final` source code of the application, so clone the `SupabaseEcommerce-starter` branch if you want to follow along with this tutorial.

```
git clone --branch SupabaseEcommerce-starter https://github.com/pramit-marattha/SupabaseEcommerce.git
```

After that, head over to the cloned directory and install the dependencies before starting the `Next.js` development server:

```bash
cd SupabaseEcommerce
yarn add all
yarn dev
```

You can now check if everything is working properly by going to `http://localhost:3000` and editing `pages/index.js`, then viewing the updated result in your browser.For more information on how to use `create-next-app`, you can review the [create-next-app documentation](https://nextjs.org/docs/api-reference/create-next-app).

![Documentation](https://user-images.githubusercontent.com/37651620/158068425-c3087d4a-14fb-46ac-a99c-10cd6412624f.png)

It usually only takes a few minutes to get everything set up. So, for this project we will be using `yarn` to add packages to a project, which will install and configure everything for us so that we can get started right away with an excellent starter template. It's time to start our development server, so head over to that `SupabaseEcommerce` folder and type `yarn add all` and then `yarn dev` and the browser will instantly open our starter template `Next.js` appplication.

![Demo](https://user-images.githubusercontent.com/37651620/158068726-0b8eafe8-6c4d-45eb-98cf-ef64faf58103.png)

Your application’s folder structure should look something like this.

![Folder structure](https://user-images.githubusercontent.com/37651620/158068978-ce6f3ba8-1571-46e9-840a-4c9a79f01666.png)

So you might be curious about the source of the content. Remember that all of our source code is housed in the pages folder, and react/next will inject it into the root div element. Let’s take a look at our pages folder, which contains some javascript files and one API folder.

![Pages](https://user-images.githubusercontent.com/37651620/158071549-8daff075-bf73-4873-96ca-ab3cd473848d.png)

Before we dive any further lets actually create a landing page for our site.

so before we even begin first you need to install `framer-motion` library.

![Framer Motion](https://user-images.githubusercontent.com/37651620/158306049-86478da8-4e06-473d-bfec-493196a228de.png)

Let's dive in and create a beautiful looking UI for our E-commerce application before we start on the backend integration part. Let's start by making a landing page for the app, and then move on to making a product page for it. So, inside the `components` folder, create a `Layout` component and add the following code to it. This component is simply a basic layout for our application that includes a navigation bar & menus as well as the functionality to display our application's registration/login modal.

```jsx
// components/Layout.js
import { Fragment, useState } from "react";
import { useRouter } from "next/router";
import Head from "next/head";
import Link from "next/link";
import Image from "next/image";
import PropTypes from "prop-types";
import AuthModal from "./AuthModal";
import { Menu, Transition } from "@headlessui/react";
import {
  HeartIcon,
  HomeIcon,
  LogoutIcon,
  PlusIcon,
  UserIcon,
  ShoppingCartIcon,
} from "@heroicons/react/outline";
import { ChevronDownIcon } from "@heroicons/react/solid";

const menuItems = [
  {
    label: "List a new home",
    icon: PlusIcon,
    href: "/list",
  },
  {
    label: "My homes",
    icon: HomeIcon,
    href: "/homes",
  },
  {
    label: "Favorites",
    icon: HeartIcon,
    href: "/favorites",
  },
  {
    label: "Logout",
    icon: LogoutIcon,
    onClick: () => null,
  },
];

const Layout = ({ children = null }) => {
  const router = useRouter();

  const [showModal, setShowModal] = useState(false);

  const user = null;
  const isLoadingUser = false;

  const openModal = () => setShowModal(true);
  const closeModal = () => setShowModal(false);

  return (
    <>
      <Head>
        <title>SupaaShop | A new way to shop!</title>
        <meta name="title" content="SupaaShopp" />
        <link rel="icon" href="/favicon.ico" />
      </Head>

      <div className="min-h-screen flex flex-col font-['Poppins'] bg-[linear-gradient(90deg, #161122 21px, transparent 1%) center, linear-gradient(#161122 21px, transparent 1%) center, #a799cc]">
        <header className="h-28 w-full shadow-lg">
          <div className="h-full container mx-auto">
            <div className="h-full px-5 flex justify-between items-center space-x-5">
              <Link href="/">
                <a className="flex items-center space-x-1">
                  <img
                    className="shrink-0 w-24 h-24 text-primary"
                    src="https://user-images.githubusercontent.com/37651620/158058874-6a86646c-c60e-4c39-bc6a-d81974afe635.png"
                    alt="Logo"
                  />
                  <span className="text-2xl font-semibold tracking-wide text-white">
                    <span className="text-3xl text-success">S</span>upabase
                    <span className="text-3xl text-success">E</span>commerce
                  </span>
                </a>
              </Link>
              <div className="flex items-center space-x-4">
                <Link href="/create">
                  <a className="ml-4 px-4 py-5 rounded-md bg-info text-primary hover:bg-primary hover:text-info focus:outline-none focus:ring-4 focus:ring-primaryfocus:ring-opacity-50  font-semibold transition">
                    Register shop !
                  </a>
                </Link>
                {isLoadingUser ? (
                  <div className="h-8 w-[75px] bg-gray-200 animate-pulse rounded-md" />
                ) : user ? (
                  <Menu as="div" className="relative z-50">
                    <Menu.Button className="flex items-center space-x-px group">
                      <div className="shrink-0 flex items-center justify-center rounded-full overflow-hidden relative bg-gray-200 w-9 h-9">
                        {user?.image ? (
                          <Image
                            src={user?.image}
                            alt={user?.name || "Avatar"}
                            layout="fill"
                          />
                        ) : (
                          <UserIcon className="text-gray-400 w-6 h-6" />
                        )}
                      </div>
                      <ChevronDownIcon className="w-5 h-5 shrink-0 text-gray-500 group-hover:text-current" />
                    </Menu.Button>
                    <Transition
                      as={Fragment}
                      enter="transition ease-out duration-100"
                      enterFrom="opacity-0 scale-95"
                      enterTo="opacity-100 scale-100"
                      leave="transition ease-in duration-75"
                      leaveFrom="opacity-100 scale-100"
                      leaveTo="opacity-0 scale-95"
                    >
                      <Menu.Items className="absolute right-0 w-72 overflow-hidden mt-1 divide-y divide-gray-100 origin-top-right bg-white rounded-md shadow-lg ring-1 ring-black ring-opacity-5 focus:outline-none">
                        <div className="flex items-center space-x-2 py-4 px-4 mb-2">
                          <div className="shrink-0 flex items-center justify-center rounded-full overflow-hidden relative bg-gray-200 w-9 h-9">
                            {user?.image ? (
                              <Image
                                src={user?.image}
                                alt={user?.name || "Avatar"}
                                layout="fill"
                              />
                            ) : (
                              <UserIcon className="text-gray-400 w-6 h-6" />
                            )}
                          </div>
                          <div className="flex flex-col truncate">
                            <span>{user?.name}</span>
                            <span className="text-sm text-gray-500">
                              {user?.email}
                            </span>
                          </div>
                        </div>
                        <div className="py-2">
                          {menuItems.map(
                            ({ label, href, onClick, icon: Icon }) => (
                              <div
                                key={label}
                                className="px-2 last:border-t last:pt-2 last:mt-2"
                              >
                                <Menu.Item>
                                  {href ? (
                                    <Link href={href}>
                                      <a className="flex items-center space-x-2 py-2 px-4 rounded-md hover:bg-gray-100">
                                        <Icon className="w-5 h-5 shrink-0 text-gray-500" />
                                        <span>{label}</span>
                                      </a>
                                    </Link>
                                  ) : (
                                    <button
                                      className="w-full flex items-center space-x-2 py-2 px-4 rounded-md hover:bg-gray-100"
                                      onClick={onClick}
                                    >
                                      <Icon className="w-5 h-5 shrink-0 text-gray-500" />
                                      <span>{label}</span>
                                    </button>
                                  )}
                                </Menu.Item>
                              </div>
                            )
                          )}
                        </div>
                      </Menu.Items>
                    </Transition>
                  </Menu>
                ) : (
                  <button
                    type="button"
                    onClick={openModal}
                    className="ml-4 px-4 py-5 rounded-md bg-info hover:bg-primary focus:outline-none focus:ring-4 focus:ring-primary focus:ring-opacity-50 text-primary hover:text-info font-extrabold transition"
                  >
                    Login
                  </button>
                )}
              </div>
            </div>
          </div>
        </header>

        <main className="flex-grow container mx-auto">
          <div className="px-4 py-12">
            {typeof children === "function" ? children(openModal) : children}
          </div>
        </main>

        <AuthModal show={showModal} onClose={closeModal} />
      </div>
    </>
  );
};

Layout.propTypes = {
  children: PropTypes.oneOfType([PropTypes.node, PropTypes.func]),
};

export default Layout;
```

Let's create a 'Hero' section of our landing page after you've successfully created a layout for the application. To do so, simply paste the following code into that section. So, in this section, we'll add an image on the right, a large text heading, and two buttons on the left.Note that we are styling our project with the absolute power of `tailwind css` and `framer-motion` to add some beautiful transition animation to the image .Since we've already created buttons on our starter template, you won't have to worry about creating them from scratch; instead, you can simply import them from the components and use them.

```jsx
// components/Hero.js
import React from "react";
import PrimaryButton from "@/components/PrimaryButton";
import SecondaryButton from "@/components/SecondaryButton";
import { motion } from "framer-motion";

const Hero = () => {
  return (
    <div className="max-w-6xl mx-auto py-12 flex flex-col md:flex-row space-y-8 md:space-y-0">
      <div className="w-full md:w-1/2 flex flex-col justify-center items-center">
        <div className="max-w-xs lg:max-w-md space-y-10 w-5/6 mx-auto md:w-full text-center md:text-left">
          <h1 className="font-primary font-extrabold text-white text-3xl sm:text-4xl md:text-5xl md:leading-tight">
            Shop <span className="text-success">whenever</span> and{" "}
            <span className="text-success">however</span> you want from,{" "}
            <span className="text-success">wherever</span> you are..{" "}
          </h1>
          <p className="font-secondary text-gray-500 text-base md:text-lg lg:text-xl">
            SuperbaseEcommerce improves and streamlines your shopping
            experience..
          </p>
          <div className="flex space-x-4">
            <PrimaryButton text="Register" link="/" />
            <SecondaryButton text="Let's Shop!" link="/products" />
          </div>
        </div>
      </div>
      <motion.div
        className="w-full md:w-1/2 transform scale-x-125 lg:scale-x-100"
        initial={{ opacity: 0, translateY: 60 }}
        animate={{ opacity: 1, translateY: 0 }}
        transition={{ duration: 0.8, translateY: 0 }}
      >
        <img
          alt="hero-img"
          src="./assets/shop.svg"
          className="mx-auto object-cover shadow rounded-tr-extraLarge rounded-bl-extraLarge w-full h-96 sm:h-112 md:h-120"
        />
      </motion.div>
    </div>
  );
};

export default Hero;
```

Now, before re-running the server, import this `Hero` component into the `index.js` file and wrap it in the Layout component to see the changes you've made.

```jsx
// index.js
import Layout from "@/components/Layout";
import Hero from "@/components/Hero";

export default function Home() {
  return (
    <Layout>
      <Hero />
    </Layout>
  );
}
```

This is how your landing page should appear.

![Demo](https://user-images.githubusercontent.com/37651620/159169803-0d0ed5ff-0084-45d8-b2b9-f802d4e69d2b.png)

After you've finished with the `Hero` section, go ahead and create a `ShopCards` component, where we'll simply list the demo features that this application offers and add some images, so your final code for the `ShopCards` component should look like this.

```jsx
// components/ShopCards.js
import React, { useState, useEffect, useRef } from "react";
import { motion } from "framer-motion";

const ShopCards = () => {
  const [tab, setTab] = useState(1);

  const tabs = useRef(null);

  const heightFix = () => {
    if (tabs.current.children[tab]) {
      tabs.current.style.height =
        tabs.current.children[tab - 1].offsetHeight + "px";
    }
  };

  useEffect(() => {
    heightFix();
  }, [tab]);
  return (
    <section className="relative">
      <div
        className="absolute inset-0 pointer-events-none pb-26"
        aria-hidden="true"
      ></div>

      <div className="relative max-w-6xl mx-auto px-4 sm:px-6">
        <div className="pt-12 md:pt-20">
          <div className="max-w-3xl mx-auto text-center pb-12 md:pb-16">
            <h1 className="text-3xl mb-4">Features</h1>
            <p className="text-xl text-gray-500">
              List of features that SuperbaseEcommerce provides.
            </p>
          </div>
          <div className="relative max-w-6xl mx-auto px-4 sm:px-6">
            <div className="pt-12 md:pt-20">
              <div className="max-w-3xl mx-auto text-center pb-6 md:pb-16">
                <div className="" data-aos="zoom-y-out" ref={tabs}>
                  <motion.div
                    className="relative w-full h-full"
                    initial={{ opacity: 0, translateY: 60 }}
                    animate={{ opacity: 1, translateY: 0 }}
                    transition={{ duration: 0.8, translateY: 0 }}
                  >
                    <img
                      alt="hero-img"
                      src="./assets/webShop.svg"
                      className="mx-auto object-cover shadow rounded-tr-extraLarge rounded-bl-extraLarge w-full h-96 sm:h-112 md:h-120"
                    />
                  </motion.div>
                </div>
              </div>
            </div>
          </div>

          <div className="max-w-6xl mx-auto py-12 flex flex-col md:flex-row space-y-8 md:space-y-0">
            <div
              className="max-w-xl md:max-w-none md:w-full mx-auto md:col-span-7 lg:col-span-6 md:mt-6 pr-12"
              data-aos="fade-right"
            >
              <div className="md:pr-4 lg:pr-12 xl:pr-16 mb-8">
                <h3 className="h3 mb-3">All of our awesome features</h3>
                <p className="text-xl text-black"></p>
              </div>
              <div className="mb-8 md:mb-0">
                <a
                  className={`flex items-center text-lg p-5 rounded border transition duration-300 ease-in-out mb-3 ${
                    tab !== 1
                      ? "bg-white shadow-md border-success hover:shadow-lg"
                      : "bg-success border-transparent"
                  }`}
                  href="#0"
                  onClick={(e) => {
                    e.preventDefault();
                    setTab(1);
                  }}
                >
                  <div>
                    <div className="font-bold leading-snug tracking-tight mb-1 text-gray-600">
                      Register/Login Feature
                    </div>
                    <div className="text-gray-600">
                      User can login and save their products for later purchase.
                    </div>
                  </div>
                </a>
                <a
                  className={`flex items-center text-lg p-5 rounded border transition duration-300 ease-in-out mb-3 ${
                    tab !== 2
                      ? "bg-white shadow-md border-purple-200 hover:shadow-lg"
                      : "bg-success border-transparent"
                  }`}
                  href="#0"
                  onClick={(e) => {
                    e.preventDefault();
                    setTab(2);
                  }}
                >
                  <div>
                    <div className="font-bold leading-snug tracking-tight mb-1 text-gray-600">
                      Add to cart
                    </div>
                    <div className="text-gray-600">
                      User can add the products/items to their cart
                    </div>
                  </div>
                </a>
                <a
                  className={`flex items-center text-lg p-5 rounded border transition duration-300 ease-in-out mb-3 ${
                    tab !== 3
                      ? "bg-white shadow-md border-purple-200 hover:shadow-lg"
                      : "bg-success border-transparent"
                  }`}
                  href="#0"
                  onClick={(e) => {
                    e.preventDefault();
                    setTab(3);
                  }}
                >
                  <div>
                    <div className="font-bold leading-snug tracking-tight mb-1 text-gray-600">
                      Security
                    </div>
                    <div className="text-gray-600">
                      Hassle free secure login and registration process.
                    </div>
                  </div>
                </a>
                <a
                  className={`flex items-center text-lg p-5 rounded border transition duration-300 ease-in-out mb-3 ${
                    tab !== 4
                      ? "bg-white shadow-md border-purple-200 hover:shadow-lg"
                      : "bg-success border-transparent"
                  }`}
                  href="#0"
                  onClick={(e) => {
                    e.preventDefault();
                    setTab(4);
                  }}
                >
                  <div>
                    <div className="font-bold leading-snug tracking-tight mb-1 text-gray-600">
                      Personalized shops
                    </div>
                    <div className="text-gray-600">
                      User can create/register their very own shop and add their
                      own products.
                    </div>
                  </div>
                </a>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>
  );
};

export default ShopCards;
```

Again, before re-running the server, import this `ShopCards` component into the `index.js` file and wrap it in the `Layout` component & below the `Hero` component to see the changes you've made.

```jsx
// index.js
import Layout from "@/components/Layout";
import Hero from "@/components/Hero";
import ShopCards from "@/components/ShopCards";

export default function Home() {
  return (
    <Layout>
      <Hero />
      <ShopCards />
    </Layout>
  );
}
```

For the time being, this is how your landing page should appear.

![Demo](https://user-images.githubusercontent.com/37651620/159170136-4c2d9b7e-dc6a-4f7d-b897-bee33ac7f338.png)

Finally, let's add a Footer section, so make a `Footer` component and paste the code below into it.

```jsx
// components/Footer.js
import Link from "next/link";

const Footer = () => {
  return (
    <footer>
      <div className="max-w-6xl mx-auto px-4 sm:px-6 pt-10">
        <div className="sm:col-span-6 md:col-span-3 lg:col-span-3">
          <section>
            <div className="max-w-6xl mx-auto px-4 sm:px-6">
              <div className="pb-12 md:pb-20">
                <div
                  className="relative bg-success rounded py-10 px-8 md:py-16 md:px-12 shadow-2xl overflow-hidden"
                  data-aos="zoom-y-out"
                >
                  <div
                    className="absolute right-0 bottom-0 pointer-events-none hidden lg:block"
                    aria-hidden="true"
                  ></div>

                  <div className="relative flex flex-col lg:flex-row justify-between items-center">
                    <div className="text-center lg:text-left lg:max-w-xl">
                      <h6 className="text-gray-600 text-3xl font-medium mb-2">
                        Sign-up for the early access!{" "}
                      </h6>
                      <p className="text-gray-100 text-lg mb-6">
                        SuperbaseEcommerce improves and streamlines your
                        shopping experience.. !
                      </p>
                      <form className="w-full lg:w-auto">
                        <div className="flex flex-col sm:flex-row justify-center max-w-xs mx-auto sm:max-w-xl lg:mx-0">
                          <input
                            type="email"
                            className="w-full appearance-none bg-purple-100 border border-gray-700 focus:border-gray-600 rounded-sm px-4 py-3 mb-2 sm:mb-0 sm:mr-2 text-black placeholder-gray-500"
                            placeholder="Enter your email…"
                            aria-label="Enter your email…"
                          />
                          <a
                            className="btn text-white bg-info hover:bg-success shadow"
                            href="#"
                          >
                            Sign-Up!
                          </a>
                        </div>
                      </form>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </section>
        </div>
        <div className="md:flex md:items-center md:justify-between py-4 md:py-8 border-t-2 border-solid">
          <ul className="flex mb-4 md:order-1 md:ml-4 md:mb-0">
            <li>
              <Link
                href="#"
                className="flex justify-center items-center text-blue-400 hover:text-gray-900 bg-blue-100 hover:bg-white-100 rounded-full shadow transition duration-150 ease-in-out"
                aria-label="Twitter"
              >
                <svg
                  className="w-8 h-8 fill-current "
                  viewBox="0 0 32 32"
                  xmlns="http://www.w3.org/2000/svg"
                >
                  <path d="M24 11.5c-.6.3-1.2.4-1.9.5.7-.4 1.2-1 1.4-1.8-.6.4-1.3.6-2.1.8-.6-.6-1.5-1-2.4-1-1.7 0-3.2 1.5-3.2 3.3 0 .3 0 .5.1.7-2.7-.1-5.2-1.4-6.8-3.4-.3.5-.4 1-.4 1.7 0 1.1.6 2.1 1.5 2.7-.5 0-1-.2-1.5-.4 0 1.6 1.1 2.9 2.6 3.2-.3.1-.6.1-.9.1-.2 0-.4 0-.6-.1.4 1.3 1.6 2.3 3.1 2.3-1.1.9-2.5 1.4-4.1 1.4H8c1.5.9 3.2 1.5 5 1.5 6 0 9.3-5 9.3-9.3v-.4c.7-.5 1.3-1.1 1.7-1.8z" />
                </svg>
              </Link>
            </li>
            <li className="ml-4">
              <Link
                href="#"
                className="flex justify-center items-center text-white hover:text-gray-900 bg-black hover:bg-white-100 rounded-full shadow transition duration-150 ease-in-out"
                aria-label="Github"
              >
                <svg
                  className="w-8 h-8 fill-current"
                  viewBox="0 0 32 32"
                  xmlns="http://www.w3.org/2000/svg"
                >
                  <path d="M16 8.2c-4.4 0-8 3.6-8 8 0 3.5 2.3 6.5 5.5 7.6.4.1.5-.2.5-.4V22c-2.2.5-2.7-1-2.7-1-.4-.9-.9-1.2-.9-1.2-.7-.5.1-.5.1-.5.8.1 1.2.8 1.2.8.7 1.3 1.9.9 2.3.7.1-.5.3-.9.5-1.1-1.8-.2-3.6-.9-3.6-4 0-.9.3-1.6.8-2.1-.1-.2-.4-1 .1-2.1 0 0 .7-.2 2.2.8.6-.2 1.3-.3 2-.3s1.4.1 2 .3c1.5-1 2.2-.8 2.2-.8.4 1.1.2 1.9.1 2.1.5.6.8 1.3.8 2.1 0 3.1-1.9 3.7-3.7 3.9.3.4.6.9.6 1.6v2.2c0 .2.1.5.6.4 3.2-1.1 5.5-4.1 5.5-7.6-.1-4.4-3.7-8-8.1-8z" />
                </svg>
              </Link>
            </li>
          </ul>

          <div className="flex-shrink-0 mr-2">
            <Link href="/" className="block" aria-label="SuperbaseEcommerce">
              <img
                className="object-cover h-20 w-full"
                src="https://user-images.githubusercontent.com/37651620/159121520-fe42bbf1-a2af-4baf-bdd8-7efad8523202.png"
                alt="SupabaseEcommerce"
              />
            </Link>
          </div>
        </div>
      </div>
    </footer>
  );
};

export default Footer;
```

> Note: Again, before re-running the server, import this `Footer` component into the `index.js` file and wrap it in the `Layout` component & below the `ShopCards` component to see the changes you've made.

```jsx
// index.js
import Layout from "@/components/Layout";
import Hero from "@/components/Hero";
import ShopCards from "@/components/ShopCards";
import Footer from "@/components/Footer";

export default function Home() {
  return (
    <Layout>
      <Hero />
      <ShopCards />
      <Footer />
    </Layout>
  );
}
```

So, if you re-run the server, this is what your application should look like.

![Demo Footer](https://user-images.githubusercontent.com/37651620/159170273-6b13b471-5d96-4e7e-88f4-9551edc253b6.png)

The structure of your component folders should resemble something like this.

![Demo](https://user-images.githubusercontent.com/37651620/159170780-1308379d-4532-4d9f-b2d7-f069b9564eac.png)

Congratulationss!! Now that you've successfully created a landing page for the application, let's move on to the core of the matter: creating the product section of the application.

So, Now let’s look at the `_app.js` file.

```jsx
// _app.js
import "../styles/globals.css";
import { Toaster } from "react-hot-toast";

function MyApp({ Component, pageProps }) {
  return (
    <>
      <Component {...pageProps} />
      <Toaster />
    </>
  );
}

export default MyApp;
```

The App component is used by `Next.js` to create pages. You can control the page initialization by simply overriding it. It allows you to do amazing things like: `Persisting layout across page changes`, `Keeping state while navigating pages`, `Custom error handling using componentDidCatch`,`Inject additional data into pages and Add global styles/CSS` are just a few of the great things you can accomplish with it.

In the above `\_app.js` code the Component parameter represents the active page, when you switch routes, Component will change to the new page. As a result, the page will receive any props you pass to Component. Meanwhile `pageProps` is an empty object that contains the initial props that were preloaded for your page by one of the data fetching methods.

Now, inside the `pages` folder, create a new page called `products.js` and import the `Layout` and `Grid` components, then import the `data.json` file as products and make the following changes to it.

```jsx
// pages/products.js
import Layout from "@/components/Layout";
import Grid from "@/components/Grid";

import products from "data.json";

export default function Products() {
  return (
    <Layout>
      <div className="mt-8 p-5">
        <Grid products={products} />
      </div>
    </Layout>
  );
}
```

## Database Configurations

Before jumping directly on our application, we'll be utilizing the power of `Supabase` to create a `PostgreSQL` database, the `Prisma schema` to define the app data model, and Next.js to connect those two together. So, let's get started building our database.

### Supabase Configurartion

Creating a PostgreSQL database in Supabase is as simple as starting a new project. Head over to [supabase.com](https://supabase.com/) and `Sign-in` to your account.

![Supabase](https://user-images.githubusercontent.com/37651620/159206560-3b46e8b2-ded2-4146-97e3-4a640b72c8b7.png)

After you've successfully signed in, you should see something similar to this.

![New project](https://user-images.githubusercontent.com/37651620/159206701-23d739cc-37d7-47b6-bf39-63c4e04c3400.png)

Now, select `New project` button. Fill in your project's required details and again click `Create Project` button and wait for the new database to load.

![Create Project](https://user-images.githubusercontent.com/37651620/159207037-87f64dbe-baf6-45b3-81c5-c9c9d9dd65c3.png)

![Creating project](https://user-images.githubusercontent.com/37651620/159207145-ef6283f0-96b2-4e24-b537-a449c8c2f8b5.png)

After the supabase configured the project, your dashboard should look something similar to this.

![SupabaseDashboard](https://user-images.githubusercontent.com/37651620/159207301-ba504656-531f-4782-a806-25b9d6169322.png)

### Creating a connection URL

Follow the steps outlined below to retrieve your database connection URL after your database has been successfully created. We'll need it to use Prisma in our Next.js app to query and create data.

- **Step1** : Head over to the `Settings tab`(Located at the left side)

![Setting Tab](https://user-images.githubusercontent.com/37651620/159232832-0ecb374c-1185-464d-b1ba-99b82c650119.png)

- **Step2** : Click the `Database` tab in the sidebar (Located on the left side)

![Database](https://user-images.githubusercontent.com/37651620/159232570-299da821-d7ec-4b35-bef8-7f6b55a86327.png)

- **Step3** : Head over to the bottom of the page to find the `Connection string` section, then select `Nodejs` and copy the URL.

![Connection string](https://user-images.githubusercontent.com/37651620/159337831-bed01290-cd64-4b48-8afa-a37fc35e2540.png)

## Initializing Prisma

Prisma is a next-generation ORM that can be used in Node.js and TypeScript applications to access a database. W eare going to use prisma fo our application because it includes all of the code we need to run our queries. It will save us a lot of time and keep us from having to write a bunch of boilerplate codes.

### Installing prisma

#### Prisma CLI installation

The Prisma command line interface (CLI) is the primary command-line interface for interacting with your Prisma project. It can create new project assets, generate Prisma Client, and analyze existing database structures via introspection to create your application models automatically.

```
npm i prisma
```

![Prisma Installation](https://user-images.githubusercontent.com/37651620/159416387-5caea0cc-d44b-4cb4-ba9d-7cc2b02a086c.png)

#### Initialize prisma

Once you've installed the Prisma CLI, run the following command to get `Prisma` started in your `Next.js` application. It will then create a `/prisma` directory and the `schema.prisma` file within it inside your particular project folder. so, inside it we will be adding all the configuration for our application.

```
npx prisma init
```

![prismaSchema](https://user-images.githubusercontent.com/37651620/159417419-f3dbe24b-0041-4306-be72-9090142d5bc3.png)

```js
// prisma.schema
generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}
```

> Note: schema.prisma uses Prisma Schema Language(PSL)

`Prisma-client-js`, the Prisma JavaScript client, is the configured client represented by the `generator` block.

```js
generator client {
  provider = "prisma-client-js"
}
```

Next one is the the provider property of this block represents the type of database we want to use, and the connection url represents how Prisma connects to it.

```js
datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}
```

### Environment variable

Using environment variables in the schema allows you to keep secrets out of the schema file which in turn improves the portability of the schema by allowing you to use it in different environments. Environment variables is created automatically after we fire the `npx prisma init` command.

> Note: Prisma supports the native connection string format for PostgreSQL, MySQL, SQLite, SQL Server, MongoDB (Preview) and CockroachDB (Preview).

```
DATABASE_URL="postgresql://test:test@localhost:5432/test?schema=foo"
```

As you can see, there is an `DATABASE_URL` variable with a dummy connection URL in this environment variable `.env`. So, replace this value with the connection string you obtained from Supabase.

```
DATABASE_URL="postgresql://postgres:[YOUR-PASSWORD]@db.bboujxbwamqvgypibdkh.supabase.co:5432/postgres"
```

### Prisma schemas and models

We can begin working on our application's data models now that database is finally connected to your `Next.js`. In Prisma, our application models should be defined within the Prisma schema using the Prisma models. These models represent the entities of our application and are defined by the model blocks in the `schema.prisma` file. Each block contains several fields that represent the data for each entity. So, let's begin by creating the `Product` model, which will define the data schema for our products properties.

#### Defining models

Models represent the entities of your application domain. Models are represented by model blocks and define a number of fields. In this data model, `Product` is the model.

```js
// prisma.schema
generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}

model Product {
  id          String   @id @default(cuid())
  image       String?
  title       String
  description String
  status      String
  price       Float
  authenticity        Int
  returnPolicy        Int
  warranty       Int
  createdAt   DateTime @default(now())
  updatedAt   DateTime @updatedAt
}
```

Each field, as shown in our Product model, has at least a name and its type. To learn more about the Scalar types and Prisma schema refrences visit the following links .

- [Data model](https://www.prisma.io/docs/concepts/components/prisma-schema/data-model#scalar-fields)
- [Prisma schema](https://www.prisma.io/docs/concepts/components/prisma-schema)
- [Prisma schema reference](https://www.prisma.io/docs/reference/api-reference/prisma-schema-reference#model-fields)

#### Generate Prisma Client

After designning Prisma model, we can begin generating our Prisma Client. We'll need to use Prisma's JavaScript library later in the article to interact with our data from within our `Next.js` app without having to write all of the SQL queries ourselves. But there's more to it. Prisma Client is, in fact, an auto-generated type-safe API designed specifically for our application which will gives us the JavaScript code we need to run queries on our data.

- **Step 1**: Installing prisma client

  ```
  npm install @prisma/client
  ```

  ![PrismaClient](https://user-images.githubusercontent.com/37651620/159432404-3e880b0a-4058-44f9-b922-652e7e389d63.png)

- **step2**: Generating Prisma client

  ```
  npx prisma generate
  ```

  ![Prisma Generate](https://user-images.githubusercontent.com/37651620/159432604-f6586811-262f-4d5f-b79b-5be9c4f1d5ff.png)

#### The @prisma/client npm package

The @prisma/client npm package consists of two key parts:

- The `@prisma/client` module itself, which only changes when you re-install the package
- The `.prisma/client` folder, which is the default location for the unique Prisma Client generated from your schema

`@prisma/client/index.d.ts` exports `.prisma/client`

Finally, after you have done that inside your `./node_modules` folder, you should now find the generated Prisma Client code.

![PrismaGenerate](https://user-images.githubusercontent.com/37651620/159435415-e4765d0e-b5c5-4fc5-9ad2-1ddf381539a4.png)

> Note: You need to re-run the prisma generate command after every change that's made to your Prisma schema to update the generated Prisma Client code.

Here is a graphical illustration of the typical workflow for the Prisma Client generation:

![WorkFlow](https://user-images.githubusercontent.com/37651620/159502666-d8c85d78-02a3-4af0-8f4e-6d29e9193247.png)

> Note also that prisma generate is automatically invoked when you're installing the `@prisma/client` npm package.

The Prisma Client is generated from the Prisma schema and is unique to your project. Each time you change the schema and run prisma generate, the client code changes itself.

![PrismaClient](https://user-images.githubusercontent.com/37651620/159506561-c361008c-6516-44fe-8d98-07ec4688cc38.png)

Pruning in `Node.js` package managers has no effect on the `.prisma` folder.

## Creating a table in `Supabase`

If you look at your database in Supabase, you'll notice there is no table inside it. It's because we haven't yet created the `Product` table.

![DashBoardScrrenshot](https://user-images.githubusercontent.com/37651620/159444118-7a373b58-b972-4bb9-9c8b-ddfd274526bd.png)

The Prisma model we defined in our `schema.prisma` file has not yet been reflected in our database. As a result, we must manually push changes to our data model to our database.

### Pushing the data model

Prisma makes it really very easy to synchonize the schema with our database.So to do that follow the command listed below.

```
npx prisma db push
```

![PrismaDB Push](https://user-images.githubusercontent.com/37651620/159443639-7b95dd91-7a02-4bd1-8603-749ccfa5a0ab.png)

![PushDatabase](https://user-images.githubusercontent.com/37651620/159445495-f28b10b6-7bb1-49d3-a6ab-e044950b51c6.png)

This command is only good for prototyping on the schemas locally.

OR,

```
npx prisma migrate dev
```

This method (`npx prisma migrate dev`) will be used in this article because it is very useful in that it allows us to directly sync our Prisma schema with our database while also allowing us to easily track the changes that we make.

So, to begin using Prisma Migrate, enter the following command into the command prompt and after that enter a name for this first migration when prompted.

![PrismaMigrate](https://user-images.githubusercontent.com/37651620/159447379-c54179e1-e1d1-4f6b-a8fc-d73059b360c4.png)

After you have completed this process successfully, prisma will automatically generate SQL database migration files, and you should be able to see the SQL which should look something like this if you look inside the `prisma` folder.

![FolderStructure](https://user-images.githubusercontent.com/37651620/159448032-a0259fb8-ed70-4275-9676-41f8b6fbc57f.png)

```sql

-- CreateTable
CREATE TABLE "Product" (
    "id" TEXT NOT NULL,
    "image" TEXT,
    "title" TEXT NOT NULL,
    "description" TEXT NOT NULL,
    "status" TEXT NOT NULL,
    "price" DOUBLE PRECISION NOT NULL,
    "authenticity" INTEGER NOT NULL,
    "returnPolicy" INTEGER NOT NULL,
    "warranty" INTEGER NOT NULL,
    "createdAt" TIMESTAMP(3) NOT NULL DEFAULT CURRENT_TIMESTAMP,
    "updatedAt" TIMESTAMP(3) NOT NULL,
    CONSTRAINT "Product_pkey" PRIMARY KEY ("id")
);
```

Finally, check the Supabase dashboard to see if everything has been successfully synced.

![Supabase](https://user-images.githubusercontent.com/37651620/159447800-82feed93-398d-4a8a-89c1-fbf10a4adf74.png)

## Prisma Studio

Prisma Studio is a visual interface to the data residing inside your database where you can use to quickly visualize and manipulate the data. The cool thing about it is that it runs in entirely on your browser and you don't need to set up any connections because it's already comes with the prisma package. Not only that, from the studio, you can quickly open all of your application's models and interact with them directly via. studio itself.

> Note: There is also desktop application available to download

### Launching Prisma Studio

Launching the prisma studio is really very easy. Literally all you have to do is run the following command from a Prisma project.

```
npx prisma studio
```

![PrismaStudio](https://user-images.githubusercontent.com/37651620/159463409-b9614ad2-5c63-4519-98f2-d2ab791cc604.png)

Now, open your browser and head over to `http://localhost:5555/`. You should be able to see the single table that we've created previously if you've followed all of the steps correctly.

![PrismaStudio](https://user-images.githubusercontent.com/37651620/159463579-2019f826-22c6-484c-a877-319ec6abe170.png)

![PrismaStudio](https://user-images.githubusercontent.com/37651620/159463739-764e709b-9ce9-469b-ada2-dce371a8efad.png)

### Manually adding the records

Lets manually add some records and save the changes that we made.

![PrismaStudio](https://user-images.githubusercontent.com/37651620/159464112-a0575030-4b00-455a-8698-48983f534d39.png)

![DataFilled](https://user-images.githubusercontent.com/37651620/159473084-776a272e-0186-42eb-8905-27e6de0ce1c4.png)

Finally, lets create a functionality to access that data from within our Next.js app, where we can create new records, update existing ones, and delete old ones.

## Interacting with data using Next.js

You should see some demo datas if you look at the `Product` page of your application.

![LandingShopButton](https://user-images.githubusercontent.com/37651620/159481862-f5f5cb2f-52f4-4b52-80a3-2cb63f41a43d.png)

![Product Page](https://user-images.githubusercontent.com/37651620/159481958-aeaab5e3-a495-47f8-acce-980701262a5f.png)

Now, open the file `pages/products.js`, file which represents our app's product page.

```js
// pages/products.js
import Layout from "@/components/Layout";
import Grid from "@/components/Grid";

import products from "products.json";

export default function Products() {
  return (
    <Layout>
      <div className="mt-8 p-5">
        <Grid products={products} />
      </div>
    </Layout>
  );
}
```

As you can see, products data is comming from `products.json` file.

```json
// products.json
[
  {
    "id": "001",
    "image": "/products/ballpen_300.png",
    "title": "Ball Pen",
    "description": "Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.",
    "authenticity": 100,
    "returnPolicy": 1,
    "status": "new",
    "warranty": 1,
    "price": 50
  },
  {
    "id": "002",
    "image": "/products/actioncamera_300.png",
    "title": "Go-pro cam",
    "description": "Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.",
    "authenticity": 100,
    "returnPolicy": 1,
    "status": "new",
    "warranty": 1,
    "price": 30
  },
  {
    "id": "003",
    "image": "/products/alarmclock_300.png",
    "title": "Alarm Clock",
    "description": "Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.",
    "authenticity": 100,
    "returnPolicy": 1,
    "status": "new",
    "warranty": 1,
    "price": 20
  },
  {
    "id": "004",
    "image": "/products/bangle_600.png",
    "title": "Bangle",
    "description": "Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.",
    "authenticity": 100,
    "returnPolicy": 1,
    "status": "new",
    "warranty": 2,
    "price": 200
  },
  {
    "id": "005",
    "image": "/products/bed_600.png",
    "title": "Large Bed",
    "description": "Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.",
    "authenticity": 100,
    "returnPolicy": 1,
    "status": "out of stock!",
    "warranty": 1,
    "price": 105
  },
  {
    "id": "006",
    "image": "/products/binderclip_600.png",
    "title": "Binder clip",
    "description": "Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.",
    "authenticity": 100,
    "returnPolicy": 2,
    "status": "new",
    "warranty": 1,
    "price": 2
  },
  {
    "id": "007",
    "image": "/products/beyblade_600.png",
    "title": "BeyBlade Burst",
    "description": "Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.",
    "authenticity": 100,
    "returnPolicy": 1,
    "status": "out of stock!",
    "warranty": 1,
    "price": 15
  },
  {
    "id": "008",
    "image": "/products/boxinggloves_600.png",
    "title": "Boxing gloves",
    "description": "Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.",
    "authenticity": 100,
    "returnPolicy": 2,
    "status": "new",
    "warranty": 1,
    "price": 45
  }
]
```

This data & information is then passed as a prop from the `Product` component to the `Grid` component. The `Grid` component is then in charge of rendering those data as a grid of Card on the screen.

```js
// Products.js
import PropTypes from "prop-types";
import Card from "@/components/Card";
import { ExclamationIcon } from "@heroicons/react/outline";

const Grid = ({ products = [] }) => {
  const isEmpty = products.length === 0;

  return isEmpty ? (
    <p className="text-purple-700 bg-amber-100 px-4 rounded-md py-2 max-w-max inline-flex items-center space-x-1">
      <ExclamationIcon className="shrink-0 w-5 h-5 mt-px" />
      <span>No data to be displayed.</span>
    </p>
  ) : (
    <div className="grid md:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-6">
      {products.map((product) => (
        <Card key={product.id} {...product} onClickFavorite={toggleFavorite} />
      ))}
    </div>
  );
};

Grid.propTypes = {
  products: PropTypes.array,
};

export default Grid;
```

Now we want to retrieve data from our database, and we'll do so using Server-Side Rendering (SSR). The ability of an application to convert HTML files on the server into a fully rendered HTML page for the client is known as server-side rendering (SSR). The web browser sends a request for information to the server, which responds immediately by sending the client a fully rendered page.

![SSR](https://user-images.githubusercontent.com/37651620/159494240-00db1ac9-7f1d-4dbe-88ef-0bdd3c196ac5.png)

So, in order to use Server Side Rendering(SSR) with `Next.js`, we must export an asynchronous function `getServerSideProps` from within the file, which exports the page where we want to render out our data. The data returned by the `getServerSideProps` function will then be used by `Next.js` to pre-render our page on each individual request. Let's get started and export this function from our applicartion's `Prodcuts` page.

```js
// pages/products.js
import Layout from "@/components/Layout";
import Grid from "@/components/Grid";

import products from "products.json";

export async function getServerSideProps() {
  return {
    props: {
      // props for the Home component
    },
  };
}

export default function Products() {
  return (
    <Layout>
      <div className="mt-8 p-5">
        <Grid products={products} />
      </div>
    </Layout>
  );
}
```

To get the data from supabase, import and instantiate the `generated Prisma client`.

```jsx
// pages/products.js
import Layout from "@/components/Layout";
import Grid from "@/components/Grid";
import { PrismaClient } from "@prisma/client";

import products from "products.json";

const prisma = new PrismaClient();

export async function getServerSideProps() {
  return {
    props: {
      // props for the Home component
    },
  };
}

export default function Products() {
  return (
    <Layout>
      <div className="mt-8 p-5">
        <Grid products={products} />
      </div>
    </Layout>
  );
}
```

Now, Using the `findMany` query, we can get all of the records in our Product table:

```jsx
// pages/products.js
import Layout from "@/components/Layout";
import Grid from "@/components/Grid";
import { PrismaClient } from "@prisma/client";

const prisma = new PrismaClient();

export async function getServerSideProps() {
  const products = await prisma.product.findMany();
  return {
    props: {
      products: JSON.parse(JSON.stringify(products)),
    },
  };
}

export default function Products({ products = [] }) {
  return (
    <Layout>
      <div className="mt-8 p-5">
        <Grid products={products} />
      </div>
    </Layout>
  );
}
```

Simply re-run the application, but if you get an error that looks like the one below, you'll need to regenerate the prisma and then re-run the server.

![Error](https://user-images.githubusercontent.com/37651620/159522918-6f09bba1-2577-4118-9ed2-b597bcb90794.png)

As you can see, its fixed now

![Fixed error](https://user-images.githubusercontent.com/37651620/159523214-7b6ff048-327d-4e71-b9f3-f5360102b6f9.png)

Finally, your application should resemble something like this:

![Application Final Demno](https://user-images.githubusercontent.com/37651620/159528273-977d0514-9618-441a-9f7e-3ad461159fa9.png)

Lets give users the functionality to actually create records from the application itself. So, first step is to actually create.

## Create a new records



---

### Chatwoot Configuration

#### Chatwoot configuration on Heroku

Let's get started by creating a chatwoot instance on Heroku.

- **Step First**: Create a free Heroku account by going to `https://www.heroku.com/` and then going to the chatwoot GitHub repository and clicking the `Deploy to Heroku` button in the readme section.

![Heroku](https://user-images.githubusercontent.com/37651620/154656511-8dfe366d-91f3-4ce6-b62f-e0685f7e5e0c.png)

![deploy](https://user-images.githubusercontent.com/37651620/154667615-9bcbba5d-55a3-4c8a-b424-1fd16f2d5a8c.png)

- **Step Second**: After you click that button, you'll be able to see the basic setup that chatwoot has already completed. Give the `App name` and replace the `FRONTEND_URL` with the `App name` you just gave, then click `Deploy App`.

![installation](https://user-images.githubusercontent.com/37651620/154667713-39521f5d-1e13-4590-b183-0e91605ea52a.png)

![URL config](https://user-images.githubusercontent.com/37651620/154667860-ec3bc9a5-1893-4c1f-a6e5-b947f53aee48.png)

- **Step Third**: Depending on your PC, network status, and server location, the program may take 10 to 15 minutes to install.

![deploying](https://user-images.githubusercontent.com/37651620/154668003-faacf254-27b6-4d62-b423-7c966c8b96fd.png)

- **Step Fourth**: After the app has been deployed, go to the settings panel in the dashboard.

![settings](https://user-images.githubusercontent.com/37651620/154668111-b7143eaa-949e-4e68-b359-2efbe96dd5f9.png)

- **Step Fifth**: The domain section can be found in the settings menu. In a new window, open that URL. Finally, you've configured chatwoot in Heroku successfully.

![domain](https://user-images.githubusercontent.com/37651620/154668214-c0cb9408-f40c-46bc-aa22-c94bb03b449b.png)

- **Step Sixth**: Inside the Resources section, make sure the `web` and `worker` resources are enabled.

![Resource section](https://user-images.githubusercontent.com/37651620/154669168-ca27814f-0246-47e7-9043-2ca2f597decc.png)

- **Step Seventh**: You should be able to log onto your chatwoot account if everything went smoothly.

![login](https://user-images.githubusercontent.com/37651620/154668422-8193c13b-c929-45f8-a2dc-9ab03d0db75d.png)

So, your first account has been created successfully.The main benefit of deploying chatwoot on Heroku is that you have full control over your entire application and your entire data.

#### Chatwoot cloud setup

There is another way to get started with [chatwoot](https://www.chatwoot.com/) which is the cloud way so this is the most straightforward way to get started is to register directly on the chatwoots [website](https://www.chatwoot.com/).

![chatwoot](https://user-images.githubusercontent.com/37651620/154645706-797c98f2-6a4b-4103-bacd-2f62e661ce2f.png)

- **Step First**: Fill out all of the required information to create an account.

![Sign up](https://user-images.githubusercontent.com/37651620/154647221-b2c786ff-becf-4793-90a3-2d407d475982.png)

- **Step Second**: You'll get an email asking you to confirm your account after you've signed up.

![Account Confirm](https://user-images.githubusercontent.com/37651620/154648298-2f3e1115-e08c-47c6-a718-f53815971f6a.png)

- **Step Third**: Proceed to login after you've confirmed your account by clicking the "Confirm my account" option.

![Login](https://user-images.githubusercontent.com/37651620/154648416-31972c34-a9e7-4c8e-b546-6f8c23a6cf73.png)

- **Step Fourth**: You may now visit the Chatwoot dashboard and begin connecting it with plethora of platform (websites, Facebook, Twitter, etc.).

![Chatwoot dashboard](https://user-images.githubusercontent.com/37651620/154648980-c1e6330a-3b59-4eae-a0a6-67a9bdb9bf3a.png)

##### Chatwoot Cloud Configuration

- **Step First**: Let's set up an inbox. The inbox channel acts as a communication hub where everything can be managed, including live-chat, a Facebook page, a Twitter profile, email, and WhatsApp.

![inbox channel](https://user-images.githubusercontent.com/37651620/154649555-f612e58f-c8f6-409f-9489-923dd21faa24.png)

- **Step Second**: Now, configure a website and domain name, as well as all of the heading and tagline information like shown below

![Website Domain](https://user-images.githubusercontent.com/37651620/154650303-51d77789-1b5e-4c0c-a6ef-183f4f37101e.png)

- **Step Third**: Finally, to control your mailbox, add "Agents." Keep in mind that only the "Agents" who have been authorized will have full access to the inbox.

![agents](https://user-images.githubusercontent.com/37651620/154650376-71e0d61c-8186-4e3f-8d25-eba2384ccde3.png)

- **Step Fourth**: Blammmm!. The website channel has been created successfully.

![website channel code](https://user-images.githubusercontent.com/37651620/154650930-c24a192f-b86a-4f6a-92eb-99222a2faecc.png)

The website channel must now be connected. Simply copy and paste the entire javascript code provided by chatwoot.Now, head back to our react app and create a new `component` folder and inside that folder create a new file/component called `ChatwootWidget` and inside it create a script which helps to loads the Chatwoot asynchronously. Simply follow the exact same steps outlined in the following code below.

```js
// ChatwootWidget.js
import { useEffect } from "react";

const ChatwootWidget = () => {
  useEffect(() => {
    // Add Chatwoot Settings
    window.chatwootSettings = {
      hideMessageBubble: false,
      position: "right",
      locale: "en",
      type: "expanded_bubble",
    };

    (function (d, t) {
      var BASE_URL = "https://app.chatwoot.com";
      var g = d.createElement(t),
        s = d.getElementsByTagName(t)[0];
      g.src = BASE_URL + "/packs/js/sdk.js";
      g.defer = true;
      g.async = true;
      s.parentNode.insertBefore(g, s);
      g.onload = function () {
        window.chatwootSDK.run({
          websiteToken: ""// add you secret token here,
          baseUrl: BASE_URL,
        });
      };
    })(document, "script");
  }, []);

  return null;
};

export default ChatwootWidget;
```

The best part about chatwoot is that you can customize it to your liking. For example, you can modify the position of the floating bubble, extend it, change the language, and hide the message bubble. All it takes is the addition of the following line of code.

```js
window.chatwootSettings = {
  hideMessageBubble: false,
  position: "right",
  locale: "en",
  type: "expanded_bubble",
};
```

Finally, it's time to import the ChatwootWidget component into our `_app_.js` file. To do so, simply navigate to the `_app_.js` file and import the chatwoot widget, then render that component. Your final code of `_app_.js` should look like this.

```jsx
// _app.js.js
import "../styles/globals.css";
import { Toaster } from "react-hot-toast";
import ChatwootWidget from "@/components/ChatwootWidget";

function MyApp({ Component, pageProps }) {
  return (
    <>
      <Component {...pageProps} />
      <Toaster />
      <ChatwootWidget />
    </>
  );
}

export default MyApp;
```

Now that you've completed the chatwoot integration, your finished project should resemble something like this.

![Demo](https://user-images.githubusercontent.com/37651620/159338768-74294b10-5ed6-474f-973c-2d26ecd1b467.png)

![Demo](https://user-images.githubusercontent.com/37651620/159338798-54c1f63b-ad86-4a7e-a2d4-587c43c98365.png)

# Conclusion

Congratulations 🎉 🎉!!. You've successfully created a fullstack application with Next.js, Supabase, Prisma and chatwoot.This article may have been entertaining as well as instructive in terms of creating a fully fgledged working ecommerce site from absolute scratch.

Aviyel is a collaborative platform that assists open source project communities in monetizing and long-term sustainability. To know more visit Aviyel.com and find great blogs and events, just like this one! Sign up now for early access, and don't forget to follow us on our socials

# Refrences

- [Managing .env files and setting variables](https://www.prisma.io/docs/guides/development-environment/environment-variables/managing-env-files-and-setting-variables#manage-env-files-manually)
- [A first look at Prisma Studio](https://daily-dev-tips.com/posts/a-first-look-at-prisma-studio/)
- [Pre-rendering and Data Fetching](https://nextjs.org/learn/basics/data-fetching/two-forms)
- [Data Model](https://www.prisma.io/docs/concepts/components/prisma-schema/data-model#scalar-fields)
- [Generating the client](https://www.prisma.io/docs/concepts/components/prisma-client/working-with-prismaclient/generating-prisma-client)
- [Instantiating the client](https://www.prisma.io/docs/concepts/components/prisma-client/working-with-prismaclient/instantiate-prisma-client)
- [Prisma schema](https://www.prisma.io/docs/concepts/components/prisma-schema)
- [Prisma schema reference](https://www.prisma.io/docs/reference/api-reference/prisma-schema-reference#model-fields)
