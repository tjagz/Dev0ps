

# SETTING UP A SIMPLE FLASK API THAT CAN CREATE RETRIEVE AND DELETE USERS


## Flask API is a lightweight framework for building web applications and RESTful APIs which supports common HTTP methods (GET, POST, PUT, DELETE) for handling different types of requests in Python using Flask. Here's adetailed step by step guide:

* Launched VSCode
* Opened my terminal on Vscode using gitbash
* Configured Flask
* Test the API using Postman




# DOCUMENTATION


## Launched VSCode


- Opened Vscode on my computer

## Opened my terminal on Vscode using gitbash

- Clicked on my terminal

- Changed the scritpting language from poweshell to ***Bash***

## Configured Flask

- Created a flask project directory and subdirectories,initial files and set up a virtual environment by using the command below;




<pre><code>


mkdir -p flask_api_project/{templates,static} && touch flask_api_project/app.py flask_api_project/templates/index.html flask_api_project/static/style.css && python -m venv flask_api_project/venv


</code></pre>






![1.0](/img6/1.0.png)









- Activated the virtual enviornment on my macOs with the command below;


***NOTE***: THIS COMMAND IS FOR MacOS USERS


<pre><code>

source venv/bin/activate  


</code></pre>



- Installed flask with the command below;

<pre><code>

pip install Flask

</code></pre>



- Changed directory to the flask_api_project directory created with the cd command;

***cd flask_api_project***


- Used the text editor on app.py to paste the code below;

***nano app.py***


<pre><code>

from flask import Flask, request, jsonify, render_template

app = Flask(__name__)

users = []

@app.route('/')
def home():
    return render_template('index.html')

@app.route('/users', methods=['POST'])
def create_user():
    user = request.get_json()
    users.append(user)
    return jsonify(user), 201

@app.route('/users', methods=['GET'])
def get_users():
    return jsonify(users), 200

@app.route('/users/<int:user_id>', methods=['GET'])
def get_user(user_id):
    user = next((u for u in users if u['id'] == user_id), None)
    return jsonify(user), 200 if user else 404

@app.route('/users/<int:user_id>', methods=['PUT'])
def update_user(user_id):
    user = request.get_json()
    index = next((i for i, u in enumerate(users) if u['id'] == user_id), None)
    if index is not None:
        users[index] = user
        return jsonify(user), 200
    return '', 404

@app.route('/users/<int:user_id>', methods=['DELETE'])
def delete_user(user_id):
    global users
    users = [u for u in users if u['id'] != user_id]
    return '', 204

if __name__ == '__main__':
    app.run(debug=True)



    </code></pre>






- Changed directory into the templates directory to access the index.html file;

***cd templates***


- Used the text editor and pasted the code below;

***nano index.html***


<pre><code>


<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>API-Based Application</title>
    <link rel="stylesheet" href="{{ url_for('static', filename='style.css') }}">
</head>
<body>
    <h1>User Management</h1>
    <form id="userForm">
        <input type="text" id="name" placeholder="Name" required>
        <input type="email" id="email" placeholder="Email" required>
        <button type="submit">Add User</button>
    </form>
    <ul id="userList"></ul>

    <script>
        document.getElementById('userForm').addEventListener('submit', async function (event) {
            event.preventDefault();
            const name = document.getElementById('name').value;
            const email = document.getElementById('email').value;
            
            const response = await fetch('/users', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify({ name, email })
            });

            const user = await response.json();
            document.getElementById('userList').innerHTML += `<li>${user.name} (${user.email})</li>`;
        });
    </script>
</body>
</html>



</code></pre>



- Cd out of the ***templates directory*** to cd into the ***Static directory*** to access the style.css

- Used the text editor on the style.css and pasted the code below;

***nano style.css***



<pre><code>

body {
    font-family: Arial, sans-serif;
    margin: 20px;
}

form {
    margin-bottom: 20px;
}

input {
    margin-right: 10px;
}


</code></pre>





- Ran the Flask application;

***flask run***




![2.0](/img6/2.0.png)


 
- Opened my browser and pasted this url http://127.0.0.1:5000 to see my application.


![7.0](/img6/7.0.png)




![7.1](/img6/7.1.png)



## Tested the API using Postman


- Installed postman on my MacOs with the command below;


<pre><code>

brew install --cask postman

</code></pre>




![3.0](/img6/3.0.png)





- After postman was successfully installed, I went on my system and searched for it in my applications


![4.0](/img6/4.0.png)



- Clicked on it to open it and clicked on continue without an account



![5.0](/img6/5.0.png)





- Clicked on open light weight API client because i didnt want to signup



![6.0](/img6/6.0.png)




- Created a new request by clicking on the dropdown arrow with Get(1) and clicked on Post(2)



![8.0](/img6/8.0.png)




- After setting the request type to POST and pasted http://127.0.0.1:5000/users, nclicked the Body tab, selected raw, and chose JSON from the TEXT dropdown.



![8.1](/img6/8.1.png)



- Entered the JSON data,clicked send and I got 201 Created response which meant a new resource was created (common for POST). 


<pre><code>


{
    "id": 1,
    "name": "Tisa",
    "email": "taykins@gmail.com"
}


</code></pre>




![8.2](/img6/8.2.png)





![9.0](/img6/9.0.png)






# END OF PROJECT






