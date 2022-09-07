
# Connect Google Form to a Node app

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

- [x] Rename downloaded `Prvate key` to `credentials.json`. Copy and paste into your node application.





