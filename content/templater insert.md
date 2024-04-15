---
title: "2024"
draft: 
tags:
  - component
---
# [How to publish Obsidian notes with Quartz on GitHub Pages](https://notes.nicolevanderhoeven.com/How+to+publish+Obsidian+notes+with+Quartz+on+GitHub+Pages)

On this page, I describe how I publish [Plain text](https://notes.nicolevanderhoeven.com/Plain+text) notes online using:

- [Obsidian](https://notes.nicolevanderhoeven.com/obsidian-playbook/Using+Obsidian/01+First+steps+with+Obsidian/Obsidian) as an interface to create and edit notes in [Markdown](https://notes.nicolevanderhoeven.com/obsidian-playbook/Using+Obsidian/02+Making+Notes+in+Obsidian/Markdown)
- [Quartz](https://notes.nicolevanderhoeven.com/Quartz) as a [Static site generator](https://notes.nicolevanderhoeven.com/Static+site+generator) to turn the Markdown into HTML
- [GitHub Pages](https://notes.nicolevanderhoeven.com/GitHub+Pages) as a hosting provider

![How to publish your notes for free with Quartz 2024-01-22 19.17.08.excalidraw.dark.png](https://publish-01.obsidian.md/access/186a0d1b800fa85e50d49cb464898e4c/plugins/Excalidraw/How%20to%20publish%20your%20notes%20for%20free%20with%20Quartz%202024-01-22%2019.17.08.excalidraw.dark.png)

This setup is free, with the exception of the last, optional, step. I'm also doing all of this on a [Mac](https://notes.nicolevanderhoeven.com/macOS). You may have to tailor terminal commands if you're using a different operating system.

Here's the end result: [Doing It in Public](https://doingitinpublic.com/). Now let's rewind and go through the steps to get there.

Much of this was taken from different pages of the [Quartz docs](https://quartz.jzhao.xyz/), but I'm reposting the steps to get this exact setup and combination of tools here.

Is this page I'm reading hosted on Quartz?

## Step 0. Prerequisites

You'll need the following installed before you continue:

- [NodeJS](https://notes.nicolevanderhoeven.com/NodeJS) v18.14+ (check your version using `node -v`)
- [NPM](https://notes.nicolevanderhoeven.com/Node+Package+Manager) v9.3.1+ (check your version using `npm -v`)
- [Git](https://notes.nicolevanderhoeven.com/Git) (check your version using `git --version`)
- [Obsidian](https://notes.nicolevanderhoeven.com/obsidian-playbook/Using+Obsidian/01+First+steps+with+Obsidian/Obsidian)

## Step 1. Download and install Quartz

### Clone the Quartz repository

Open up your terminal and run this command:

```
git clone https://github.com/jackyzha0/quartz.git
```

This downloads a copy of the Quartz repository and stores it locally on your computer, in a folder called `quartz`.

Rename the `quartz` folder to something more descriptive by running:

```
mv quartz my-notes
```

where `my-notes` is the name of your site.

### Install Quartz dependencies

Quartz is a Node project, and it requires other libraries to run.

Change the directory to the new folder:

```
cd my-notes
```

Now, use NPM to install those dependencies:

```
npm i
```

### Initialize Quartz

Time to create a new Quartz project! Run:

```
npx quartz create
```

You are then asked to choose between three different methods of initializing the content in your directory:

```
- Empty Quartz
- Copy an existing folder
- Symlink an existing folder
```

Use the arrow keys to select `Empty Quartz`, and then hit Enter. You should then see something like this:

```
Choose how Quartz should resolve links in your content. You can change this later in `quartz.config.ts`.
- Treat links as absolute path
- Treat links as shortest path
- Treat links as relative paths
```

What you select here is dependent on how you prefer to handle links in Obsidian. By default, Obsidian uses the shortest path where possible, so unless you know you'll want to change this behaviour later, use the arrow keys to select the second option, `Treat links as the shortest path`, and then hit Enter.

If you already have an Obsidian vault, and you want to check which one you're using...

You should see something like this:

```
You're all set! Not sure what to try next? Try:
- Customizing Quartz a bit more by editing `quartz.config.ts`
- Running `npx quartz build --serve` to preview your Quartz locally
- Hosting your Quartz online (see: https://quartz.jzhao.xyz/hosting)
```

## Step 2. Set up a GitHub repository

### Create a GitHub repository

Now you need to synchronize the local Quartz repository that you've cloned to a [remote](https://notes.nicolevanderhoeven.com/Working+with+remotes+in+Git) repository on [GitHub](https://notes.nicolevanderhoeven.com/GitHub). GitHub is a hosting provider for repositories. [Create an account if you don't already have one](https://github.com/) - it's free.

When logged into GitHub, click on the _New_ button to create a new repository.

In _Repository name:_, type `my-notes` (or whatever you chose in [Step 1](https://notes.nicolevanderhoeven.com/How+to+publish+Obsidian+notes+with+Quartz+on+GitHub+Pages#Step%201.%20Download%20and%20install%20Quartz)).

For _Initialize this repository with:_ Make sure that _Add a README file_ is NOT ticked.

Click _Create repository_ at the end.

### Change the `origin` remote

A remote is a name for a repository hosted on GitHub that you can refer to later. There are already two remotes that are set up, but you need to change one of them.

On GitHub, after creating your repository, you should see a screen like this:

![github-quick-setup.png](https://publish-01.obsidian.md/access/186a0d1b800fa85e50d49cb464898e4c/assets/github-quick-setup.png)

HTTPS vs. SSH?

In the screenshot, I chose `SSH`. [SSH](https://notes.nicolevanderhoeven.com/SSH) is more secure, but it has its disadvantages. Unless you have a preference, I recommend you choose [HTTPS](https://notes.nicolevanderhoeven.com/HTTPS).

Click on the Copy button to copy what's in the text field.

Then, on your terminal, run:

```
git remote -v
```

You should see something like:

```
origin  https://github.com/jackyzha0/quartz.git (fetch)
origin  https://github.com/jackyzha0/quartz.git (push)
upstream        https://github.com/jackyzha0/quartz.git (fetch)
upstream        https://github.com/jackyzha0/quartz.git (push)
```

which shows two remotes, `origin` and `upstream`, each of which has a `fetch` and a `push`.

Now run:

```
git remote rm origin
```

This command removes the `origin` remote.

Now add the remote back, but this time with the correct repository:

```
git remote add origin https://github.com/yourusername/my-notes.git
```

where `yourusername` is the username you selected for GitHub and `my-notes` is the name of the vault you chose in [Step 1](https://notes.nicolevanderhoeven.com/How+to+publish+Obsidian+notes+with+Quartz+on+GitHub+Pages#Step%201.%20Download%20and%20install%20Quartz).

Check the remotes again:

```
git remote -v
```

You should see something like:

```
origin  https://github.com/yourusername/my-notes.git (fetch)
origin  https://github.com/yourusername/my-notes.git (push)
upstream        https://github.com/jackyzha0/quartz.git (fetch)
upstream        https://github.com/jackyzha0/quartz.git (push)
```

Note the changes in the first two lines.

### Sync your changes

In this section, you are pushing the changes you made to your local repository (`my-notes` on your computer) to your remote repository (`my-notes` on GitHub).

Run:

```
npx quartz sync --no-pull
```

You should get some text back with a nice green `Done!` at the end.

To verify that it's worked, go back to GitHub and refresh that page you were on where you previously copied that text string containing your repository's address. You should see something like this there instead:

![github-quartz-sync.png](https://publish-01.obsidian.md/access/186a0d1b800fa85e50d49cb464898e4c/assets/github-quartz-sync.png)

Your repositories are synced! From now on, every time you run that last command, any changes you've made locally will be sent to your GitHub repository.

## Step 3. Create an Obsidian vault

### Opening your Quartz repository in Obsidian

Time to start actually creating content! You're going to do that in Obsidian. So fire up Obsidian, and when asked which vault to open, click on _Open_ next to _Open folder as vault._

A Finder dialog window pops up. Navigate to the folder you created (`my-notes`) and then click _Open_.

### (Optional) Setting up Obsidian

At this point, you can spend some time to set up Obsidian the way you like-- or you can skip this step entirely. To give you an idea, here are the things I did:

- I copied over the `.obsidian/hotkeys.json` from my main vault to my new vault, so that all my keyboard shortcuts will work in the new one too.
- Selected a theme.
- Installed and set up plugins I knew I'd want in the new vault.

### Create a note template in Obsidian

Quartz needs certain [properties](https://notes.nicolevanderhoeven.com/obsidian-playbook/Obsidian+Plugins/Core+Plugins/Properties+in+Obsidian) in the [YAML Frontmatter](https://notes.nicolevanderhoeven.com/obsidian-playbook/Using+Obsidian/03+Linking+and+organizing/YAML+Frontmatter) to work. You _could_ just type these out each time, but I highly suggest using a template for this. You can use either the [core plugin](https://notes.nicolevanderhoeven.com/obsidian-playbook/Obsidian+Plugins/Core+Plugins/Core+plugins) [Templates](https://notes.nicolevanderhoeven.com/obsidian-playbook/Obsidian+Plugins/Core+Plugins/Templates), or you could use the community plugin [Templater](https://notes.nicolevanderhoeven.com/obsidian-playbook/Obsidian+Plugins/Community+Plugins/Templater+plugin). I chose Templater, so install and enable that before continuing.

In Obsidian, create a note in `templates` called `note` (or whatever you want it to be called). In that note, copy this:

```
---
title: "How to publish Obsidian notes with Quartz on GitHub Pages"
draft: false
tags:
  - 
---
 

```

Go to Settings > Templater. In _Template folder location_, type `templates`.

### Create content

In Obsidian, create a new note or two with your new template.

## Step 4. Link your local files to GitHub

### Build your site locally

Before you publish your site online, you should verify that it all works on your computer. This requires "building" the site by running a Quartz server that will convert the Markdown to HTML so that you can see it on your web browser.

Then, in your terminal, run this command:

```
npx quartz build --serve
```

You should see a block of text that includes this line:

```
Started a Quartz server listening at http://localhost:8080
```

Open up your web browser and type in `http://localhost:8080`.

You should see your site rendered in your browser:

![quartz-local-build.png](https://publish-01.obsidian.md/access/186a0d1b800fa85e50d49cb464898e4c/assets/quartz-local-build.png)

The page shown may look different if you changed its contents, but the important thing is that you see your changes reflected in your browser.

### Sync to GitHub

Now that you've verified that your site renders correctly locally, it's time to sync your changes to GitHub.

Run this command:

```
npx quartz sync
```

This command sends your changes to your online GitHub repository.

## Step 5. Host your vault online

At this point, you are able to view a site (the rendered HTML version of your Markdown notes) locally, but when you sync those changes to GitHub, they're still just in the repository. In this step, you'll set your repository up so that those Markdown files get generated and published online.

### Create a `deploy.yml` file

From your terminal, run:

```
mkdir .github/workflows/deploy.yml
```

Open up that file that you just created in Finder.

Make sure hidden files are visible

`.github` is a hidden file, so if you don't see it, hit `CMD + OPT + .` so that you do see it.

The blank file should have opened in your default text editor. Now copy and paste this into it:

```
name: Deploy Quartz site to GitHub Pages
 
on:
  push:
    branches:
      - v4
 
permissions:
  contents: read
  pages: write
  id-token: write
 
concurrency:
  group: "pages"
  cancel-in-progress: false
 
jobs:
  build:
    runs-on: ubuntu-22.04
    steps:
      - uses: actions/checkout@v3
        with:
          fetch-depth: 0 # Fetch all history for git info
      - uses: actions/setup-node@v3
        with:
          node-version: 18.14
      - name: Install Dependencies
        run: npm ci
      - name: Build Quartz
        run: npx quartz build
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v2
        with:
          path: public
 
  deploy:
    needs: build
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v2
```

(_Note: This was updated on 2024-01-25. If you're reading this much later than that, get the latest version of this from [the Quartz documentation](https://quartz.jzhao.xyz/hosting#github-pages)._)

Save the file.

Why can't you see this file in Obsidian?

### Create a GitHub Action

In GitHub, go to Settings > Pages.

Under _Source_, select _GitHub Actions_.

Then, go back to your terminal and sync. Remember, that's:

```
npx quartz sync
```

### Behold your new site!

Verify that this worked by visiting `https://yourusername.github.io/my-notes` from your browser.

You should see your site displayed, just as you saw it when you built it locally.

Wait... is this really free?

You can stop here if you're happy with the new URL above. But what if you already have a nice shiny new domain and you want your site to be hosted there instead?

## (Optional) Use a custom domain for your new site

This is the only part that is not free. You'll need to pay for your domain. You can buy one at a domain registrar. I like [Porkbun](https://notes.nicolevanderhoeven.com/Porkbun).

### Create A records for your domain

On your domain registrar, navigate to the DNS records section for your domain. On Porkbun, [go to the Domain Management page](https://porkbun.com/account/domainsSpeedy), hover over your domain, and then click the _DNS_ link that appears below it.

Select _A - Address record_ under _Type_.

In _Answer_, paste this:

```
185.199.108.153
```

Here's what that looks like in Porkbun:

![quartz-porkbun-add-a-records.png](https://publish-01.obsidian.md/access/186a0d1b800fa85e50d49cb464898e4c/assets/quartz-porkbun-add-a-records.png)

Click Add.

Do this three more times, adding these three more A records for your domain, one by one:

```
185.199.109.153
185.199.110.153
185.199.111.153
```

This step makes it so that when someone goes to your domain, they're served up content from the GitHub servers (at those IP addresses).

### Set up the custom domain on GitHub Pages

Go back to GitHub and navigate to your repository.

Go to Settings > Pages.

In _Custom domain_, enter your domain name, like `supercooldomain.com`. Click _Save_.

### Wait

Actually, don't wait. Go for a walk, make a hot beverage of your choice, sleep. Sometimes, DNS changes take ages to propagate.

You can check whether the changes have propagated by going to `supercooldomain.com`. You should see your Quartz site there.

Go back to Settings > Pages on GitHub and under the _Custom domain_ section, tick the option _Enforce HTTPS._ Now you should be able to go to `https://supercooldomain.com`, too!

## References

- [Quartz documentation](https://quartz.jzhao.xyz/)
- [GitHub documentation](https://docs.github.com/)
- [GitHub Pages documentation](https://docs.github.com/en/pages)
- [Obsidian documentation](https://help.obsidian.md/Home)

[Changelog](https://notes.nicolevanderhoeven.com/Changelog)

[Quartz](https://notes.nicolevanderhoeven.com/Quartz)