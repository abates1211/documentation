# Auto-Garcon REST API

Written By Tyler Beverley, Sosa Edison. 
This is a living document that will act as the documentation for the API that will be used by Auto-Garcon. Be sure to check this document often as it will change as the API changes. There are several sections in this document, the first of which details how to send requests to the API. frequently used datastructures and details their inner components. The name of the data structure might be used as short hand further in the document. The next section details the various endpoints that are available. 

Naming of JSON feilds follows cammelCase convention. 

## Connecting

The base URL to send requests to is: [TBD].  
 
The API accepts GET and POST requests. POST parameters are to be in JSON format. GET requests will be used for retreiving data (reading) and POST for creating, updating, and removing data.  

## Data Structures 
Various data structures and enums that will be used commonly in the API. Enums are denoted by curly braces, and optional arguments are surrounded in brackets. Anything followed by an astrerix indicates it is likely to change. Time is writen in 24 hour time (i.e Midnight = 2400). Types are denoted after the variable name and a colon.  
  
MenuType = { Dinner, Breakfast, Brunch, Breakfast, Lunch, Drink }  
OrderStatus = { Open, Closed }  
MenuStatus = { Draft, Active, Deleted \* }  
Category = Any User Defined String  
Allergens = { Meat, Diary, Nuts, Gluten, Nuts, Soy, Other } `
  
* _MenuItem_
  * itemID : int
  * itemName : text
  * description : text
  * status : bit
  * calories : int
  * category : string
  * gluten : bit
  * meat : bit
  * dairy : bit
  * nuts : bit
  * soy : bit

* _Menu_  
  * menuID : int
  * startTime : int (ex. 1200)
  * endTime : int (ex. 1300)
  * menuStatus : int
  * menuName : string
  * restaurantID : int
  * menuImage : text
  * type : string

* _Order_
  * orderID : int 
  * tableID : int
  * orderTime : dateTime
  * status : string
  * chargeAmount : float int
  * resturantID : int 
  * customerName : string
  * alexaStatus : bit
  
* _Restaurant_
  * restaurantID : int
  * restaurantName : string
  * description : text
  * address : text
  * salesTax : float int
  * city : string
  * state : string
  * zipCode : string
  * country : string
  
* _OrderItem_
  * orderItemID : int
  * menuItemID : int
  * quantity : int
  * comments : text
  * orderID : int
  
* _MenuContains_
  * menuID : int
  * menuItemID : int
  * price : double int
  

## Endpoints 

This section details the various endpoints available on the API. All characters in the URL will be lowercase. A part of a route that is lead by a colon indicates a parameter in the URL. So /users/:userid will look like /users/12314. Feilds with no data type specifed are inferred. All endpoints will respond with a HTTP status code and any specifed JSON response. 
  
The documentation follows this style: 

* parent route
 * HTTP Method: full route  
   * query paramaters in the format of: ?param=[options]
   * Request JSON paramaters for POST requests. 
   * Response JSON
  
---  


* /users
   * GET /users/:userid 
    * Response: 
      * userID
      * firstName
      * lastName
   * GET /users/:userid/favorites
     * ?resturantid=int 
     * Response: 
       * numResturants
       * resturants[] 
         * resturantID
         * resturantName
         * favorites: Items[] 
   * GET /users/:userid/orders
      * ?range=int
      * Response: Order Structure
   * POST /users/newuser 
     * Request: 
       * firstName
       * lastName
       * email
     * Response: 
       *  userID
     
      
* /restaurant   
  * GET /restaurant/:restaurantid
    * Response:  
      * resturantID
      * resturantName
      * resturantAddress
      * availableMenuTypes: MenuType[]
  * GET /restaurant/:restaurantid/orders
    * ?status=[open, closed, all]
    * ?userid=int
    * Response:
      * Orders: OrderStructure[]
  * GET /restaurant/:restaurantid/menu
  	* ?type=MenuType
    * Response: 
       * menu : Menu
  * GET /restaurant/:restaurantid/tables
  * POST /restaurant/sitdown
    * QRcode : bytes 
    * Used to identify what restaurant and table the user is at.
    * Response: 
      * restaurantID
      * restaurantName
      * tableNumber
      * userID
  * POST /restaurant/:restaurantid/menu/add
    * Request: 
      * menu : Menu
    * Reponse: 
      * menuID
  * POST /restaurant/:restaurantid/menu/remove
     * Request: menuID
  * POST /restaurant/:restaurantid/orders/sumbit
    * Request:
      * customerID
      * chargeAmount
      * tableID
      * items : MenuItems[] 
      * customizations : text
    * Response: 
      * orderID 
  * POST /restaurant/:restaurantid/orders/complete
     * orderID
     
     
    

Notes:
-----
Android Will work on menu stuff first
Stripe integration will need parameters added to some endpoints. 

Database team will use auto-incrementing unique IDs for items, restaurants and users

How will database handle images? 
Stored bytes directly? Third party service and just store links? 
