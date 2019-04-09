# cfg-flask-heroku-guide
## Step 1: Modify your python file
This assumes you have a Flask application already created. 
For this demonstration, I create a very simple Flask application in `hello.py`. 
This file is very similar to what we saw in the lecture. 
Please not the important parts that may be different to yours: at the top of the file you need to write `import os`,
and at the bottom of the file instead of having simply `app.run(debug=True)`, replace that with the following:  
```
if __name__ == "__main__":
    port = int(os.environ.get('PORT', 5000))
    app.run(debug=True, host='0.0.0.0', port=port)
```
This piece of code will tell our Flask application which port to bind to, this is where our browser will be pointed to to find our application.

## Step 2: Specify the requirements for heroku
Now we need to create the file that tells Heroku which libraries to install. Remember how you had to do `pip install flask`?
Heroku needs to be able to do the same thing on their end. To do this, we create a file called `requirements.txt`, and we do this
by running `pip freeze > requirements.txt` in the terminal inside of our project folder. This file has to be at the top level of
your project directory!

## Step 3: Tell Heroku which version of Python to run
Since there are many versions of Python, Heroku needs to know which one to use. Create a file called `runtime.txt` in the same
level as `requirements.txt`, and in this file simply write `python-2.7.15`.

## Step 4: Tell Heroku how to run your application
Now, we need to tell Heroku what it needs to do to run our Flask application. We do this by creating a file called `Procfile` (note the capital)
next to our other files we just created in this guide. Inside of this file we write: `web: python hello.py`. In my case, my flask application
is inside of the python file called `hello.py`, this may be different for you so simply replace that with the name of your python file.

## Step 5: Commit and run
Finally, now that we have made all of these changes, we need to commit them, and push them to Heroku.
Lets start by adding all of the files we just changed/added to our next commit (again, replace `hello.py` with your file's name):  
`$ git add hello.py Procfile runtime.txt requirements.txt`  

Now commit this:  
`$ git commit -m "Added the stuff to make this run on heroku"`  

And now push this to Heroku:  

`$ git push heroku master` 


Hopefully that should push your project up to Heroku, and Heroku will run your flask application :)  

Note: There is a slight chance you should run `$ heroku ps:scale web=1`. Also, a useful command if your website crashes, you can run `$ heroku logs --tail` to see what went wrong on the heroku side :)
