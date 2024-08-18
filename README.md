This is an [AstroPaper](https://vercel.com/templates/astro/astro-paper) based on [Astro](https://astro.build) framework.

## Getting Started
To create your own page:

```bash
# npm 6.x
npm create astro@latest --template satnaing/astro-paper

# npm 7+, extra double-dash is needed:
npm create astro@latest -- --template satnaing/astro-paper

# yarn
yarn create astro --template satnaing/astro-paper

# pnpm
pnpm dlx create-astro --template satnaing/astro-paper
```

First, run the development server:

```bash
npm start
```

or

```bash
# Build the Docker image
docker build -t astropaper .

# Run the Docker container
docker run -p 4321:80 astropaper
```

Open [http://localhost:4321](http://localhost:4321) with your browser to see the
result.

You can start editing the page by modifying `src/content`. The page
auto-updates as you edit the file.


## Deploy on Vercel

The easiest way to deploy is to use the
[Vercel Platform](https://docs.astro.build/en/guides/deploy/vercel/)
from the creators of Astro.
