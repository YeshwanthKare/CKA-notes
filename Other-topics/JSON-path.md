### JSON Path

#### Objectives

- YAML
- YAML vs JSON
- JSON PATH
  - Dictionaries
  - Lists
  - Lists and Dictionaries
  - Criteria
- Practice Exercises

---

#### Root Element

- The Root Element is denoted by a '$'

```
{
	"car": {
		"color": "blue",
		"price": "$20,000"
	},
	"bus": {
		"color": "white",
		"price": "$120,000"
	}
}

$.car for accesing car
$.bus for accessing bus
```

- If the JSON has vehicles

```
{
	"vehicles":{
		"car": {
			"color": "blue",
			"price": "$20,000"
		},
		"bus": {
			"color": "white",
			"price": "$120,000"
		}
	}
}

$.vehicles.car for accessing car
$.vehicles.bus for accessing bus
```

#### JSON PATH - Lists

- For example, we have the JSON like below

```
{
	"car": {
		"color": "blue",
		"price": "$20,000",
		"wheels": [
			{
				"model": "X345ERT",
				"location": "front-right"
			},
			{
				"model": "X346GRX",
				"location": "front-left"
			},
			{
				"model": "X236DEM",
				"location": "rear-right"
			},
			{
				"model": "X987XMV",
				"location": "rear-right"
			}
		]
	}
}

$.car.wheels[1].model
```

#### JSON PATH - Criteria

- we are willing to get the numbers greater than 40

```
[
	12,
	43,
	23,
	12,
	56,
	43,
	93,
	32,
	45,
	63,
	27,
	8,
	78
]

# Get all numbers greater than 40
$[ Check if each item in the array > 40 ]

Check if => ? ()

$[?( each item in the list > 40 )]

each item in the list => @

$[?(@ > 40)]

# Like this certain ways can be used

@ == 40
@ != 40
@ in [40,43,45]
@ nin [40,43,45]
```

- If we wanted to specify the item by a certain criteria, we can use the above mentioned rule, i.e., searching the location by rear-right

```
$.car.wheels[?(@.lcation == "rear-right")].model
```

- The command to extract the values in a keypair in command line

```
{
	"property1": "value1",
	"property2": "value2"
}

cat q1.json | jpath '$.property1'
```

#### JSON PATH - Wild card

- To get the colors but not just color of the vehicle, we can use '\*' for this purpose

```
{
	"car": {
		"color": "blue",
		"price": "$20,000"
	},
	"bus": {
		"color": "white",
		"price": "$120,000"
	}
}

# To get all colors
$.*.color


# To get all prices
$.*.prices
```

- Wheel example

```
[
	{
		"model": "X345ERT",
		"location": "front-right"
	},
	{
		"model": "X346GRX",
		"location": "front-left"
	},
	{
		"model": "X236DEM",
		"location": "rear-right"
	},
	{
		"model": "X987XMV",
		"location": "rear-right"
	}
]


# To get the model of all wheels
$[*].model
```

- To get the details of the models of all vehicles at a time

```
{
	"car": {
		"color": "blue",
		"price": "$20,000",
		"wheels": [
			{
				"model": "X345ERT",
			},
			{
				"model": "X346GRX",
			}
		]
	},
	"bus": {
		"color": "white",
		"price": "$120,000",
		"wheels": [
			{
				"model": "X345ERT",
			},
			{
				"model": "X346GRX",
			}
		]
	},
}

-> $.car.wheels[0].model
-> $.car.wheels[*].model
-> $.bus.wheels[*].model
-> $.*.wheels[*].model
```
