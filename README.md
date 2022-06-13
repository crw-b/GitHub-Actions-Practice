# GitHub-Actions-Practice
A sample repo to practice using GitHub Actions for Javascript

Set-Up
The first part of this set-up is taken from Ania Kubów. If you'd rather follow along with a YouTube tutorial, please watch her video here: https://www.youtube.com/watch?v=COPS4VMfaUc


1. Fork and clone this repo onto your local machine.

2. Move into the GitHub-Actions-Practice directory you created and make a new package.json for this project by typing the following into your terminal:

/////
   $ npm init -y

3. Create a directory for your actions by typing the following into your terminal:

/////
   $ touch action.yml

4. Install @actions/github by entering into your terminal:

////
   $ npm install @actions/github

5. Install @actions/core by entering into your terminal:

////
   $ npm install @actions/core

6. Open up the action.yml file and paste the following sample code from GitHub Docs:

/////

    name: 'Hello World'
    description: 'Greet someone and record'
    inputs:
        who-to-greet: # id of input
        required: true
        default: 'World'
    outputs:
        time: # id of output
        description: 'The time we greeted you'
    runs:
        using: 'node12'
        main: 'index.js'
//////

Notice that both who-to-greet and time are variables that are set to the id of input and output, respectively. We are able to use variables here because we downloaded @actions/core allowing multiple action files to interact.

6. Since the action we just created runs on 'index.js', we will need to add an index.js file to our directory.

7. Call @actions/core and @actions/github within index.js with the following code at the top of the file:

////
const core = require('@actions/core');
const github = require('@actions/github');

8. Copy and paste the sample code into your new index.js file.

//////////

    // this is the file that will be run by action.yml
    try {
        // `who-to-greet` input defined in action metadata file action.yml
        const nameToGreet = core.getInput('who-to-greet');
        console.log(`Hello ${nameToCreet}!`);
        const time = (new Date()).toTimeString();
        core.setOutput("time", time);
        // Get the JSON webhook payload for the event that triggered the workflow
        const payload = JSON.stringify(github.context.payload, undefined, 2)
        console.log(`The event payload: ${payload}`);
    } catch (error) {
        core.setFailed(error.message);
    }

////////

9. Add, commit, and push your changes to your forked repo.


10. Then, you will want to tag this action so you can reference it later. Type the following into your terminal with any message you'd like (the v1 is what we'll actually reference the tag with):

///////
$ git tag -a -m 'SOME MESSAGE' v1
//////
$ git push --follow-tags
//////

11. Create a folder named '.github/workflows' and create a file inside named 'main.yml'. This will be a path for you to access your workflows from your local. The following instructions will only work for a public github repo.

12. Inside of main you will paste the following code:

////

 on: [push]

jobs:
  hello_world_job:
    runs-on: ubuntu-latest
    name: A job to say hello
    steps:
    - name: Hello world action step
      id: hello
      uses: YOUR-GITHUB-USERNAME/github-actions-practice@v1
      with:
        who-to-greet: 'YOUR NAME HERE'
    # Use the output from the `hello` step
    - name: Get the output time
      run: echo "The time was ${{ steps.hello.outputs.time }}"

/////

Be sure to replace 'YOUR-GITHUB-USERNAME' with your github username so that the action can follow the correct path and you can replace YOUR NAME HERE with whatever variable you want to pass through.

12. 