# ZSSN (Zombie Survival Social Network)

### Problem Description

ZSSN (Zombie Survival Social Network). The world as we know it has fallen into an apocalyptic scenario. A laboratory-made virus is transforming human beings and animals into zombies, hungry for fresh flesh.
You, as a zombie resistance member (and the last survivor who knows how to code), was designated to develop a system to share resources between non-infected humans.
Requirements
You will develop a REST API which will store information about the survivors, as well as the resources they own.

In order to accomplish this, the API must fulfill the following use cases:
● Add survivors to the database

 A survivor must have a name, age, gender and last location (latitude, longitude).

 A survivor also has an inventory of resources of their own property (which you need to declare upon the registration of the survivor).

● Update survivor location

 A survivor must have the ability to update their last location, storing the new latitude/longitude pair in the base (no need to track locations, just replacing the previous one is enough).

● Flag survivor as infected

 In a chaotic situation like that, it's inevitable that a survivor may get contaminated by the virus. When this happens, we need to flag the survivor as infected.

 An infected survivor cannot trade with others, can't access/manipulate their inventory, nor be listed in the reports (infected people are kinda dead anyway, see the item on reports below).

 A survivor is marked as infected when at least three other survivors report their contamination.

 When a survivor is infected, their inventory items become inaccessible (they cannot trade with others).

● Survivors cannot Add/Remove items from inventory

 Their belongings must be declared when they are first registered in the system. After that they can only change their inventory by means of trading with other survivors.

 The items allowed in the inventory are described below.

● Trade items:

 Survivors can trade items among themselves.

 To do that, they must respect the price table below, where the value of an item is described in terms of points.

 Both sides of the trade should offer the same amount of points. For example, 1 Water and 1 Medication (1 x 4 + 1 x 2) is worth 6 ammunition (6 x 1) or 2 Food items (2 x 3).

 The trades themselves need not to be stored, but the items must be transferred from one survivor to the other.

| Item | Points |
| ------ | -----|
| Water | 4 | 
| Food | 3 |
| Medication | 2 |
| Ammuntion | 1 |

● Reports

 The API must offer the following reports:

1. Percentage of infected survivors.
2. Percentage of non-infected survivors.
3. Average amount of each kind of resource by survivor (e.g. 5 waters per survivor)
4. Points lost because of infected survivors.


## Requirements

* [Python](https://www.python.org/downloads/release/python-3107/) (3.10.7)
* [Django](https://docs.djangoproject.com/pt-br/2.0/) (3.2.10)
* [Django Rest Framework](http://www.django-rest-framework.org/)
* [Model Mommy](http://model-mommy.readthedocs.io/en/latest/basic_usage.html) - Old version not maintained 
* [Model Bakery](https://pypi.org/project/model-bakery/)- New Version i.e maintained

## HOW TO RUN

Follow the below commands to setup the project 

1. Install the dependencies (requirements.txt is provided for persual)

2. Run the following 

```
python manage.py makemigrations restApi
python manage.py migrate restApi
python manage.py loaddata restApi/fixture/default.json

```

3. To run the server:

```
python manage.py runserver

```
4. To run the tests:

```
python manage.py test

```
## Routes

1. To register a new survivor.
   - METHOD: POST
   - ENDPOINT: api/v1/survivor/
   - Request Example: 

Adding survivor to DB

![Adding survivor to DB](https://user-images.githubusercontent.com/55132850/215311258-904183d5-4d5f-4385-90a8-107607178343.png)

```
{
            "name": "Ashish",
            "age": 26,
            "gender": "M",
            "latitude": 28.598428,
            "longitude": 77.087658,
            "is_infected": false,
            "count_reports": 0,
            "inventory": {
                "inventory_items": [
                    {"id": 1},
                    {"id": 2},
                    {"id": 3},
                    {"id": 4}
                ]
            }
        }

```
Survivor added to DB (SURVIVOR TABLE)

![Survivor added to DB](https://user-images.githubusercontent.com/55132850/215311675-081dd56b-2d87-4305-a393-907eff56f91b.png)


NOTE: The table shows the value of the ids. We have the score that each survivor will have when owning the item. 

   ITEM TABLE 
   
   ![item table](https://user-images.githubusercontent.com/55132850/215313274-92a651ce-121d-414a-b7e9-10ec3e43532a.png)


2. To update a survivor location.
   - METHOD: PUT
   - ENDPOINT: api/v1/survivor/{id_survivor}/
   - Request Example: 

Updating Location

![Updating Location](https://user-images.githubusercontent.com/55132850/215311283-9ce88e73-2c04-439a-894a-172c98813db2.png)

```
{
    "longitude": 9945672735,
    "latitude": 958345622
}

```
Location updated in DB

![location updated in DB](https://user-images.githubusercontent.com/55132850/215311295-08a1e456-4a67-4ada-a7f4-db4e62ae311d.png)


3. To report a survivor as infected.
   - METHOD: PUT
   - ENDPOINT: api/v1/survivor/report_infection/{id_survivor_infected}/
   - Request Example: 
   
   Reporting Survivor as infected (Flagged)
   
   ![Reporting Survivor as infected (Flagged)](https://user-images.githubusercontent.com/55132850/215311366-7994396e-9308-4952-ab43-48110a9a7644.png)

   Reporting Survivor as infected (Flagged) DB View
   
   ![Reporting Survivor as infected (Flagged) DB View](https://user-images.githubusercontent.com/55132850/215311375-2a200970-5815-4afd-91dc-b4ec53883d00.png)

   
4. To trade items between survivors.
   - METHOD: PUT
   - ENDPOINT: api/v1/survivor/trade_items/{survivor1_id}/{item1}/{survivor2_id}/{item2}
   - Request Example:

   Trade between survivors
   
   ![trade between survivors](https://user-images.githubusercontent.com/55132850/215312647-5b779dd2-5b3d-4038-b912-f67dae3b7ffe.jpg)
  
   ITEMS BEFORE TRADE (INVENTORY ITEAMS TABLE)
   
   ![inventory items before trade](https://user-images.githubusercontent.com/55132850/215313058-0804a5f4-cea1-4070-aa3b-79d8a43818ad.jpg)

   ITEMS AFTER TRADE (INVENTORY ITEAMS TABLE)
   
   ![inventory items after trade](https://user-images.githubusercontent.com/55132850/215312664-ad3c4b0c-6dd4-4a96-8b16-abb62de43619.jpg)
   
   **NOTE: The resources should follow the pattern `count-resource-count-resources-..` (e.g. 1-ammunition-1-food or 1-water)**

5. To get percentage of infected survivors.
   - METHOD: GET
   - ENDPOINT: /api/v1/survivor/survivors_infected/
   - Request Example:
   
   ![percentage of infected survivors](https://user-images.githubusercontent.com/55132850/215311966-cec40155-9092-48b5-8e99-994314176029.png)

   
6. To get percentage of non-infected survivors.
   - METHOD: GET
   - ENDPOINT: /api/v1/survivor/survivors_no_infected/
   - Request Example:
   
   ![percentage of noninfected survivors](https://user-images.githubusercontent.com/55132850/215312176-cf271c20-5cff-42a5-a54d-3d958137b7fc.png)


7. To get average amount of each kind of resource by survivor.
   - METHOD: GET
   - ENDPOINT: /api/v1/survivor/avg_items/
   - Request Example:
   
   ![average amount of each kind of resource](https://user-images.githubusercontent.com/55132850/215312024-60a5d593-fb79-493f-a378-4280ed92e580.png)

   
8. To get total number of points lost because of infected survivors.
   - METHOD: GET
   - ENDPOINT: /api/v1/survivor/points_lost/
   - Request Example:
   
   ![Total points lost](https://user-images.githubusercontent.com/55132850/215312052-80765202-41a5-4e43-9d15-abb09992ae8c.png)


9. To get points lost because of a infected survivor.
   - METHOD: GET
   - ENDPOINT: /api/v1/survivor/points_lost/{id}
   - Request Example:
   
   Points lost because of infected survivor flagged 5 times
   
   ![Points lost because of infected survivor flagged 5 times](https://github.com/Ashishkumar-hub/Zombie-Survival-Social-Network-NL/blob/main/images/points%20lost%20because%20of%20infected%20survivor%20flagged%205%20times.png)
   
   Points lost because of infected survivor
   
   ![Points lost because of infected survivor](https://github.com/Ashishkumar-hub/Zombie-Survival-Social-Network-NL/blob/main/images/points%20lost%20because%20of%20infected%20survivor.png)

  
  


