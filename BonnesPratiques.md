# Importer les fichiers csv ci-joint dans votre base de données.
```
mongoimport --type csv -d ecomDb -c allsales --headerline --drop All-sales.csv
mongoimport --type csv -d ecomDb -c categories --headerline --drop categories.csv
mongoimport --type csv -d ecomDb -c customers --headerline --drop customers.csv
mongoimport --type csv -d ecomDb -c employees --headerline --drop employees.csv
mongoimport --type csv -d ecomDb -c employees --headerline --drop employees.csv
mongoimport --type csv -d ecomDb -c employees --headerline --drop employees.csv
mongoimport --type csv -d ecomDb -c employees --headerline --drop employees.csv
mongoimport --type csv -d ecomDb -c employees --headerline --drop employees.csv
mongoimport --type csv -d ecomDb -c employeeterritories --headerline --drop employee-territories.csv
mongoimport --type csv -d ecomDb -c orderdetails --headerline --drop order-details.csv
mongoimport --type csv -d ecomDb -c regions --headerline --drop regions.csv
mongoimport --type csv -d ecomDb -c products --headerline --drop products.csv
mongoimport --type csv -d ecomDb -c shippers --headerline --drop shippers.csv
............
```
# Vérifier que les restrictions de nommage de mongoDB sont respectées et listez
```
> use stock_db
switched to db ecomDb
> db.getName()
stock_db
> db.getCollectionNames()
[
        "allsales",
        "categories",
        "customers",
        "employees",
        "employeeterritories",
        "orderdetails",
        "orders",
        "products",
        "regions",
        "shippers",
        "suppliers",
        "territories"
]
```
# Créer des règles de validation
```
db.allsales.validate();
db.categories.validate()
.....
```

```
 $jsonSchema{
...     bsonType: "object",
...     required:[
...       'OrderID',
...       'CustomerID',
...       'EmployeeID',
...       'OrderDate',
...       'ShippedDate',
...       'ShipName',
...       'ShipAddress',
...       'ShipCity',
...       'ShipPostalCode',
...       'ShipCountry'
...       ],
...       properties:{
...         orderID:{
...           bsonType: 'int'
...         },
...         CustomerID:{
...           bsonType:'string',
...           description: 'must be a string and is required'
...         },
...         EmployeeID:{
...           bsonType:'int'
...         },
...         ShipName:{
...           bsonType: 'string'
...         },
...         ShipAddress:{
...           bsonType: 'string'
...         },
...         ShipPostalCode:{
...           bsonType:'int'
...         }
...
...       }
...   }
```
# Activer les control d’accès et exiger une authentification.
#  Configurer des rôles.

```
use admin
switched to db admin
db.createUser(
...   {
...     user: "admin",
...     pwd: "admin",
...     roles: [ { role: "userAdminAnyDatabase", db: "admin" }, "readWriteAnyDatabase" ]
...   }
... )
Successfully added user: {
        "user" : "admin",
        "roles" : [
                {
                        "role" : "userAdminAnyDatabase",
                        "db" : "admin"
                },
                "readWriteAnyDatabase"
        ]
}

```
# Créer des utilisateurs dans les règles de l’art des normes du sécurité (e.g. des utilisateurs qui administre les utilisateurs et d’autre qui administrent les bases de données) et affecter à eux les rôles convenables. Vous devez faire des veilles TEC sur database fine tuning, sharding, replication et capped collections sous MongoDB.



```
use customers
db.createUser(
 {
  user: "zakaria",
  pwd: "123456",
  roles: [
   { role: "read", db: "Products" },
   { role: "readWrite", db: "Categories" }
  ]
 }
)
```
```
Successfully added user: {
        "user" : "zakaria",
        "roles" : [
                {
                        "role" : "read",
                        "db" : "Products"
                },
                {
                        "role" : "readWrite",
                        "db" : "Categories"
                }
        ]
}
```
```
quit()
```
```
mongo --username zakaria --password --authenticationDatabase stock_db --authenticationMechanism SCRAM-SHA-256
```
