# Signal Check (general version) - deployment guide

This is the standalone version: job legitimacy check for candidates, posting grader
for employers. No CV/letter generation, no "Post it," no lookup counter - those live
in the Enhancv-specific version.

## What you need before starting

1. A free [Vercel](https://vercel.com) account (sign up with GitHub - fastest option).
2. An Anthropic API key from [console.anthropic.com](https://console.anthropic.com) -
   this is billed to your own Anthropic account, separate from any Claude.ai subscription.

## Steps

### 1. Get your files onto GitHub
Create a new GitHub repository and push this whole folder to it (or use GitHub's
"upload files" button in the browser if you don't want to use git directly).

### 2. Import the project into Vercel
- Go to vercel.com -> "Add New" -> "Project" -> import the GitHub repo you just created.
- Framework preset: choose "Other."
- Click Deploy. It will succeed even without the API key set yet, but scans won't
  work until you add it (next step).

### 3. Add your API key
- In the Vercel project -> Settings -> Environment Variables.
- Add a variable named `ANTHROPIC_API_KEY` with your real key as the value.
- Redeploy (Vercel -> Deployments -> three dots on the latest one -> Redeploy) so the
  function picks up the new variable.

### 4. Test it
Visit the URL Vercel gives you. Run a real scan on both tabs. If something's wrong,
check Vercel -> your project -> Deployments -> the latest one -> Functions tab for
logs from `/api/claude`.

## What changed from the version tested inside Claude

The scan now calls `/api/claude`, a serverless function that holds the real API key
server-side, instead of calling Anthropic directly from the browser. That's the only
structural change - everything else behaves the same.

## Honest limitations, unchanged from before

- No caching - every scan is a fresh, billed API call with up to 3 web searches.
- No rate limiting. If you're sending this to real people, keep an eye on your
  Anthropic usage dashboard, especially early on.
- Small companies with generic names can still get missed by search if no company
  website is given - see the "Company website" field, which helps this a lot.
