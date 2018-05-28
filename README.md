# Equella Web components

Equella integration Web Components for the uPortal ecosystem.

You will need Node 8+ and Git 1+ to use this project.

## Using the Build System

### Initial Setup

After cloning, run the following command once to download project dependencies.

```console
npm install
```

### Run Locally

Use the following command to start the application locally for development and testing.

```console
npm run serve
```

### Build for Deployment

Use the following command to build Equella Web components for distribution and deployment in uPortal.

```console
npm run build
```

## Running in uPortal

After building, copy all the files in the `dist/` directory into
`uPortal-start/overlays/uPortal/src/main/webapp` (or other location served by Tomcat).

:notebook: At some point this project will build to a Maven artifact that can be bundled with
uPortal-start independantly.

## Components

### Equella Search Results

Displays the results of a pre-configured search query attractively in the portal.

#### Sample HTML

Define a SimpleCMS portlet with HTML content like the following:

```HTML
<script src="https://unpkg.com/vue@2.5.16"></script>
<script type="text/javascript" src="/uPortal/equella.js"></script>
<equella-search-results msg="Equella Searching Demo" search-result-types="mini courses" base-url="https://equella.unicon.net/demo-oa18-up-eq-integ" collection-id="1ffa9507-4313-4f4f-9f6c-b923a897c515" result-icon="_THUMBS/promo.jpeg_PREVIEW_.jpeg" result-landing-page="index.html"/>
```

### Equella Image Carousel

Displays an image carousel in the portal using attachments created and managed in Equella.

#### Sample HTML

Define a SimpleCMS portlet with HTML content like the following:

```HTML
<script src="https://unpkg.com/vue@2.5.16"></script>
<script type="text/javascript" src="/uPortal/equella.js"></script>
<equella-image-carousel base-url="https://equella.unicon.net/demo-oa18-up-eq-integ" search-tag="EVENT_2017_04_CONCERTS" result-image="_THUMBS/promo.jpeg_PREVIEW_.jpeg" result-landing-page="index.html"/>
```
