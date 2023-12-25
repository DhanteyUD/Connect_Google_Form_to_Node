## Connect Google Form to a Node.js app

Connecting a Google Form to a Node app using Google API and retrieving information stored in Google sheet

## Creating a Google form and a Google sheet:

- [x] Access Google  `Forms` and click on *Blank* to create new or edit existing form 


![create-form](https://user-images.githubusercontent.com/85023604/188122587-bab7a280-33c1-48cb-9e22-97e10499ee64.jpg)

- [x] Send out forms to be filled. When successful, go to the `Responses` and create a `Spreadsheet`

![Spreadsheet](https://user-images.githubusercontent.com/85023604/188124438-7d12607f-bb3e-4925-9500-6b60485c5fbc.jpg)

> Spreedsheet basically holds all data submitted to a form. 

![Spreedsheet](https://user-images.githubusercontent.com/85023604/188126081-784fbe20-1585-4751-80bb-19436ece6595.jpg)

## Creating a Google Service Account:

- [x] Go to <a href="https://console.cloud.google.com">`https://console.cloud.google.com`</a>

- [x] Create a `NEW PROJECT` (if you don't have one already)

![Screen Shot 2022-09-05 at 3 10 40 PM 1](https://user-images.githubusercontent.com/85023604/188469345-46fd776c-5b88-45a3-93ad-16df19016f4d.png)

- [x] Enter your `Project Name` and `Location` and click the `CREATE` button

![Screen Shot 2022-09-05 at 3 18 29 PM](https://user-images.githubusercontent.com/85023604/188470231-3121fd61-ab89-4f7a-8056-30ece74d535e.png)

- [x] Select your project

![Screen Shot 2022-09-05 at 3 23 07 PM](https://user-images.githubusercontent.com/85023604/188471123-3d7063a5-b1a2-45a9-88dd-b7ec90c1d9a8.png)

- [x] Enable the sheet API

> To do this, click on the menu icon -> `APIs & Services` -> `Library` -> Search for the sheet API using `spreadsheet` and you will get the below result:

![Screen Shot 2022-09-07 at 2 55 37 PM](https://user-images.githubusercontent.com/85023604/188896978-205c1f04-276c-483c-ad0a-e8ec3f8cbf5f.png)

![Screen Shot 2022-09-07 at 2 57 12 PM](https://user-images.githubusercontent.com/85023604/188897081-d18d1936-454d-42be-b4a1-9e574292dfac.png)

- [x] In order to get your credentials, navigate to the dashboard by Clicking the `menu icon` -> `APIs & Services` -> `Dashboard` - `Credentials`. Click on `CREATE CREDENTIALS`

![Screen Shot 2022-09-07 at 3 06 39 PM](https://user-images.githubusercontent.com/85023604/188900029-2f37ed68-a6e4-4972-bfeb-7ba03c17abd3.png)

- [x] After clicking on `CREATE CREDENIALS` -> select `Service Account Key` (for `Key type`, select `JSON`) -> For your `Service account`, enter your desired name,  select your role, enter a service account ID and then click on `CREATE`.

> After creating, the `Private Key` would be saved to your computer. 

- [x] Rename downloaded `Private key` to `credentials.json`. Copy and paste into your node application.

## Using `credentials.json` in node app and connecting to Google Sheet service:

 - [x] Install bootstraped express application using `npm install express-generator` or `yarn add express-generator`

- [x] install `nodemon` using `npm install nodemon` or `yarn add nodemon`

> Start your server and get your node app running.

### To utilize the google sheet service:

1. Go to the `app.js` file and import dependencies for the Google Sheets API

- [x] Import `fs` 
- [x] Import `google` from `googleapis`
- [x] Initialize `service` from `google sheet v4`
- [x] Import `credentials` from project root folder

```js
  const fs = require('fs'); // This would be used to write our sheet data to an output JSON file
  const { google } = require('googleapis'); // This would be used to connect to the Google API
  const service = google.sheets('v4'); // This is to enable our node app utilize the spreedsheet service
  const credentials = require('./credentials.json'); // This would be used to authenticate and authourize user access to the spreedshet data
```
2. Configure the Google auth client

```js
  const authClient = new google.auth.JWT(
    credentials.client_email,
    null,
    credentials.private_key.replace(/\\n/g, '\n'),
    ['https://www.googleapis.com/auth/spreadsheets']
  );
```
3. Create a function and write the logic to get the googlesheet data as soon as it is saved on the sheet.

```js
const formData = async () => {
  try {
    
    const token = await authClient.authorize(); // Authorize the client

    authClient.setCredentials(token);  // Set the client credentials

    // Get the rows
    const res = await service.spreadsheets.values.get({
      auth: authClient,
      spreadsheetId: credentials.spreadsheetId, // This is the id from your Google sheet
      range: 'A:D', // This indicates the number of columns needed to be shown
    });
 ```
  ![{5D80D875-D217-4872-90EC-1D4ED8B6651C} png](https://user-images.githubusercontent.com/85023604/190859916-cb95fe1e-1d57-44f1-be25-fded0b913601.jpg)
  
  ```js
    // An Output array to hold all data
    const Output = [];

    // Set rows to equal the rows
    const rows = res.data.values;

    // Check if we have any data and if we do, add it to our Output array
    if (rows.length) {
      // Remove the headers
      rows.shift();

      // For each row
      for (const row of rows) {
        Output.push({
          TimeStamp: row[0],
          Name: row[1],
          Email: row[2],
          InitialApplications: row[3],
          CV: row[4],
        });
      }
    } else {
      console.log('No data found.');
    }

    // Save output data to a Output.json file
    fs.writeFileSync(
      'Output.json',
      JSON.stringify(Output, null, 2),
      function (err, file) {
        if (err) throw err;
        console.log('Saved!');
      }
    );
  } catch (error) {
    console.log(error);

    // Exit the process with error
    process.exit(1);
  }
};

formData();
```
4. Go to your `package.json` file and add a `script` for starting your server using `nodemon`

> Under `scripts`, add 
> ```js
> {
>  "dev": "nodemon ./bin/www"
> }
> ```

![{F8BD913A-8394-4D8B-AAD6-608D3BE7A104} png](https://user-images.githubusercontent.com/85023604/190860490-c1fbfa91-adbd-49d9-960a-1c4fc2665f76.jpg)

5. Run `npm run dev` to start server

> Wheneven the google form is filled and submitted, the data is automatically saved to googlesheet and our node app immediately converts the `CSV` data to a `JSON` format and saves it to the `Output.json` file.
