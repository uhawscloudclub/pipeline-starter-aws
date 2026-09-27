# My First CI/CD Pipeline on AWS

Every time you push a change, a pipeline checks your code. Only if the checks pass does it publish your website.
Everything happens in your browser. No installs.

## Before the workshop (do this at home)
- Sign in to GitHub once on the laptop you'll bring. Have your 2FA app or recovery codes ready.
- Create your AWS account at least 2 days early. Sign in once and open the Amplify console to make sure it's active.
- Turn on MFA for your AWS root user, and create a zero-spend budget in **Billing and Cost Management > Budgets**.
- Charge your laptop.

If your AWS account isn't working on the day, pair up with a classmate and follow along on their screen.

## Step 1. Copy the starter repo
1. On the workshop repo page, click the green **Use this template** button, then **Create a new repository**.
2. Name it `my-first-pipeline`. Set it to **Public**. Click **Create repository**.

## Step 2. Connect it to AWS Amplify
1. Sign in to the AWS Console. In the top-right corner, set the Region to **US East (N. Virginia)**.
2. Search for **Amplify** and open it. Click **Deploy an app**.
3. Choose **GitHub**, then **Next**. Two GitHub screens will appear:
   - **Authorize AWS Amplify**: click **Authorize**.
   - **Install AWS Amplify**: choose **Only select repositories**, pick `my-first-pipeline`, click **Install & Authorize**.
   - Nothing appeared? Your browser blocked the pop-up. Allow pop-ups for this site and try again.
4. Pick your repo and the `main` branch. Click **Next**.
5. Amplify should say it found `amplify.yml`. Leave everything else as is. Click **Next**, then **Save and deploy**.
6. Wait for **Deployed** (1-3 minutes). Click the link. That's your live site.

## Step 3. Make your first change
1. In your GitHub repo, click `index.html`, then the pencil icon.
2. Change `[Your Name]` to your name.
3. Click **Commit changes**, then **Commit changes** again.
4. In Amplify, watch the new build run. Refresh your site when it finishes.

## Step 4. Break it on purpose: a bug
1. Edit `index.html`. Delete this whole line: `<title>My First Pipeline</title>`. Commit.
2. Watch the build **fail**. Open the build log and search (Ctrl+F) for **FAILED**.
3. Your site is still up and still showing your last working version. The pipeline protected it.
4. Put the line back exactly as above. Commit. Watch it go green.

## Step 5. Break it on purpose: a leaked key
1. Copy the **fake** example key from the workshop slide. Never paste a real key anywhere.
2. Edit `index.html` and paste it anywhere in the page. Commit.
3. Watch the build fail with **FAILED - leaked AWS key found**. Your site does not update.
4. Remove the key and commit. Back to green.

**The part that matters:** removing the key did not un-leak it. It is still in your repo's history, and the repo is public. With a real key, the first move is to **deactivate the key in AWS**, then clean up the code. This check only stops a bad deploy. Real teams add secret scanners like GitHub push protection, gitleaks, or TruffleHog, and use short-lived credentials so there's no key to leak.

## Bonus challenges
- Put the key in a **new file** (like `config.js`). Is it caught? Look at `amplify.yml` to see why.
- Add a third check: the page must contain your name.
- Try to sneak a key past the check. What does that tell you about grep-based scanning?

## Put this on your résumé and LinkedIn

You built something real today. Here's how to show it without overselling it.

**Before you post, make it yours**
- Put your name on the page (Step 3) and keep the Amplify link working.
- Add one line to the top of this README: what the project is and your live site link.
- Recruiters click links. A live site plus a repo with a green build history beats any adjective.

**Résumé (Projects section)**

> **CI/CD Pipeline on AWS** | GitHub, AWS Amplify, YAML | [live site link]
> - Built a CI/CD pipeline that automatically tests and deploys a website to AWS Amplify Hosting on every push to GitHub
> - Configured automated quality and secret-scanning checks that block deployment when they fail, and verified them by pushing a broken page and a simulated leaked AWS access key

Did a bonus challenge? Add a third bullet for it, for example:
> - Wrote a custom build check that verifies required page content before every deploy

**LinkedIn (Profile > Add section > Projects)**
- **Name:** CI/CD Pipeline on AWS
- **Skills:** CI/CD, AWS Amplify, GitHub, DevSecOps
- **Link:** your live site and this repo
- **Description:**
> Built a CI/CD pipeline at the AWS Student Builder Group at University of Houston. Every push to GitHub runs automated checks in AWS Amplify, and the site only deploys if they pass. I tested it by pushing a broken page and a simulated leaked AWS key, and watched the pipeline block both. Biggest takeaway: a pipeline can stop a bad deploy, but a leaked key is still leaked, so the real fix is revoking it.

**Rules of thumb**
- Only claim what you did. If you paired with a classmate, rebuild it on your own account first (about 15 minutes with this README).
- Use numbers and results ("blocked 2 failing deploys"), not "familiar with" or "exposure to".
- Be ready to explain it in an interview: what CI and CD mean, what happens when a check fails, and why deleting a leaked key isn't enough.

## Clean up
Keep your Amplify app running if you link it on your résumé. Heads-up: an AWS Free plan account closes after 6 months unless you upgrade, and your live link goes with it. To remove the app, delete it in the Amplify console.
