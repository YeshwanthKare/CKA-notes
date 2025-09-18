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

#### JSON Path lists

- To get the elements in the list

```
[
	"Apple",
	"Google",
	"Microsoft",
	"Amazon",
	"Facebook",
	"Coca-Cola",
	"Samsung",
    "Disney",
	"Toyota",
	"McDonald's"
]
```

- Queries

```
$[0] - To get the first element in the list

$[3] - To get the 4th element

$[0,3] - To get the 1st and the 4th element

$[0:3] - To get the 1st to 4th elements, but doesn't include the 4th element

$[0:4] - If we wanted to have the 4th element

$[0:8] - gets all the 8 elements in the list

$[0:8:2] - it is a step which fetches the step wise manner, like get 1st, 3rd, 5th & 7th in a list, in other words skip or hop over one other item

$[-1] - Gets the last item in the list, but doesnot work in certain implementations, instead we can use the term below

$[-1:0] - gets the last element
$[-1:]

$[-3:] - Gets the last three items
```

#### JSON PATH in Kubernetes

- JSON path with Kubectl utility

Objectives:
- JSON PATH in Kubectl
- Why JSON PATH?
- View and interpret Kubectl output in JSON Format
- How to use JSON PATH with Kubectl
- JSON PATH examples
- Loops - Range
- Custom Columns
- Sort

- Why JSON Path

1. Large Data sets
	- 100s of Nodes
	- 1000s of PODs, Deployments, ReplicaSets

##### Kubectl

- kubectl get nodes => This command is queried by the Kube-apiserver and then returns the human readable format
- kubectl get nodes -o wide => to see the other options in the command

##### Kubectl - JSON PATH

- What if I wanted to see if the name of the nodes and the CPU they use like below

```
NAME      CPU
master    4
node01    4

        or

NAME      TAINTS
master    node-role.kubernetes.io/master
node01

        or

NAME       ARCHITECTURE
master     amd64
node01     amd64
```
- None of the builtin commands can give us this
- With JSON PATH we can filter the output of the command as we like

##### How to JSON PATH in Kubectl?

1. Identify the Kubectl command
	- kubectl get nodes
	- kubectl get pods

2. Familiarize with JSON output
	- kubectl get nodes -o json
	- kubectl get pods -o json

3. Form the JSON PATH query
	- .items[0].spec.containers[0].image

4. Use the JSON PATH query with kubectl command
	- kubectl get pods -o=jsonpath='{ .items[0].spec.containers[0].image }'

##### JSON PATH examples

```
kubectl get nodes -o=jsonpath='{ .items[*].metadata.name }'
master   node01

kubectl get nodes -o=jsonpath='{ .items[*].status.nodeInfo.architecture }'
amd64    amd64

kubectl get pods -o=jsonpath='{ .items[*].status.capacity.cpu }'
4   4

# Merging two queries into one

kubectl get nodes -o=jsonpath='{ .items[*].metadata.name }{ .items[*].status.capacity.cpu }'

# layout of the JSON PATH

kubectl get nodes -o=jsonpath='{ .items[*].metadata.name }{"\n"}{ .items[*].status.capacity.cpu }'
master   node01
4         4

{"\n"} - New line
{"\t"} - Tab
```

##### Loops - Range

```
kubectl get nodes -o=jsonpath='{ .items[*].metadata.name }{"\n"}{ .items[*].status.capacity.cpu }'

# Range

'{range .items[*]}

	{.metadata.name}{"\t}{.status.capacity.cpu}{"\n"}

{end}'

# we can merge it all and pass it onto a JSON PATH kubectl command

kubectl get nodes -o=jsonpath='{range .items[*]}{.metadata.name}{"\t}{.status.capacity.cpu}{"\n"}{end}'
```

##### JSON PATH for Custom Columns

```
kubectl get nodes -o=custom-columns=<COLUMN NAME>:<JSON PATH>

kubectl get nodes -o=custom-columns=NODE:.metadata.name ,CPU:.status.capacity.cpu

NODE     CPU
master   4
node01   4
```

##### JSON PATH for Sort

```
kubectl get nodes --sort-by=.metadata.name

kubectl get nodes --sort-by=.status.capacity.cpu
```