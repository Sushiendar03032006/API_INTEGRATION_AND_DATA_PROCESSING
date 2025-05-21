## Exercise_11-API_Integration_and_Data_Processing
# Aim:
To create a workflow in UiPath that calls a REST API (e.g., Weather API), processes the JSON response, and writes the extracted data to a database.

## Equipment Required:
UiPath Studio<br>
API Key from a Weather API provider (e.g., OpenWeatherMap)<br>
Database (e.g., SQL Server, MySQL, SQLite)<br>
## Procedure:
### Step 1: Install Required Packages
Open UiPath Studio.<br>
Go to Manage Packages in the top toolbar.<br>
#### Install the following packages:<br>
UiPath.WebAPI.Activities (to call REST APIs)<br>
UiPath.Database.Activities (to interact with databases)<br>
### Step 2: Obtain the Weather API Details
For this example, we’ll use the OpenWeatherMap API:<br>

API Endpoint:<br>
```
https://api.openweathermap.org/data/2.5/weather?q={city_name}&appid={your_api_key}
```
Replace {city_name} with the desired city (e.g., "London") and {your_api_key} with your personal API key from OpenWeatherMap.

### Step 3: Building the UiPath Workflow
#### 1. Create a New Sequence
Open UiPath Studio and create a new sequence called WeatherAPI_to_DB.
#### 2. Add HTTP Request Activity
Search for HTTP Request activity from the activities panel.<br>
Configure it as follows:<br>
Endpoint (URL):<br>
```
https://api.openweathermap.org/data/2.5/weather?q=London&appid=YOUR_API_KEY
```
Method: Set it to GET as we are retrieving data.<br>
Output: Create a variable called responseJSON to store the API response.
#### 3. Deserialize JSON
Use the Deserialize JSON activity to convert the JSON response into a UiPath-readable format.<br>
Input: responseJSON<br>
Output: Create a variable jsonResponse to store the deserialized data.
#### 4. Extract Required Data
Extract specific information like city name, temperature, and humidity from the JSON response.<br>
Use Assign activities to extract values:<br>
city = jsonResponse("name").ToString<br>
temperature = jsonResponse("main")("temp").ToString<br>
humidity = jsonResponse("main")("humidity").ToString<br>
#### 5. Connect to Database
Use Connect activity from the UiPath.Database.Activities package.<br>
Connection String: Based on the type of database you're using (e.g., SQLite, MySQL). Here’s an example for SQLite:<br>
```
Data Source=path_to_your_database;Version=3;
```
Store the connection in a variable called dbConnection.
#### 6. Insert Data into Database
Use Execute Non Query activity to write data into the database.<br>
SQL Query:<br>
```
INSERT INTO WeatherData (City, Temperature, Humidity)
VALUES (@City, @Temperature, @Humidity)
```
In the Parameters section, assign:<br>
@City -> city<br>
@Temperature -> temperature<br>
@Humidity -> humidity
#### 7. Close Database Connection
Use Disconnect activity to close the database connection after writing the data.
## UiPath WorkFlow:
![alt text](<img/Screenshot 2024-10-12 112403.png>)
![alt text](<img/Screenshot 2024-10-12 112445.png>)
![alt text](<img/Screenshot 2024-10-12 112514.png>)
![alt text](<img/Screenshot 2024-10-12 112619.png>)
![alt text](<img/Screenshot 2024-10-12 112713.png>)
![alt text](<img/Screenshot 2024-10-12 112745.png>)
![alt text](<img/Screenshot 2024-10-12 112813.png>)
![alt text](<img/Screenshot 2024-10-12 113157.png>)
![alt text](<img/Screenshot 2024-10-12 113241.png>)
![alt text](<img/Screenshot 2024-10-12 113334.png>)
![alt text](<img/Screenshot 2024-10-12 182140.png>)
![alt text](<img/Screenshot 2024-10-12 182320.png>)
![alt text](<img/Screenshot 2024-10-12 182418.png>)
![alt text](<img/Screenshot 2024-10-12 182502.png>)
## Result:
The UiPath workflow successfully calls the weather API, processes the JSON response, and writes data such as city name, temperature, and humidity into the specified database table.
