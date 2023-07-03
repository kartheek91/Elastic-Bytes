# Combinational Queries

## Write and execute a search query that is a Boolean combination of multiple queries and filters

Question 1 (using must):
What is the salary, years of experience, and age of a Software Engineer with a Bachelor’s degree who is male? <br>
## Query: <br>
Option-1
```
{
  "query": {
    "constant_score": {
      "filter": {
        "bool": {
          "must_not": [
            {
              "term": {
                "Gender": "Female"
              }
            },
            {
              "term": {
                "Job Title": "Data Analyst"
              }
            },
            {
              "term":{
                "Education Level":"Master's"
              }
            }
          ]
        }
      }
    }
  }
}

```
Better Version
```
GET salaries/_search
{
  "_source": [
    "Salary",
    "Years of Experience",
    "Age"
  ],
  "query": {
    "constant_score": {
      "filter": {
        "bool": {
          "must": [
            {
              "term": {
                "Job Title": "Software Engineer"
              }
            },
            {
              "term": {
                "Education Level": "Bachelor's"
              }
            },
            {
              "term": {
                "Gender": "Male"
              }
            }
          ]
        }
      }
    }
  }
}
```
<br>
Question 2 <br>
Which job title, salary, and years of experience do not belong to a female Data Analyst with a Master's degree? 
<br>

```
GET salaries/_search
{
  "query": {
    "constant_score": {
      "filter": {
        "bool": {
          "must_not": [
            {
              "term": {
                "Gender": "Female"
              }
            },
            {
              "term": {
                "Job Title": "Data Analyst"
              }
            },
            {
              "term":{
                "Education Level":"Master's"
              }
            }
          ]
        }
      }
    }
  }
}
```
<br>
Question 3 <br>
What are the details (salary, years of experience, education level, gender, job title, and age) of individuals who either have an education level of PhD or a job title of Software Engineer? 
<br>

```
GET salaries/_search
{
  "query": {
    "constant_score": {
      "filter": {
        "bool": {
          "should":[
              {
                "term":{
                  "Education Level" : "PhD"
                }
              },
              {
                "term":{
                  "Job Title" : "Software Engineer"
                }
              }
            ]
        }
        
      }
    }
  }
}
```
<br>
Question 4 
<br>
Which job titles belong to individuals with a salary greater than $80,000, who have a Bachelor's or PhD degree, and are either male or have an age less than 30  but exclude those with a job title of "Senior Manager"?
<br>

```
GET salaries/_search
{
  "query": {
    "constant_score": {
      "filter": {
        "bool": {
          "must": [
            {
              "term": {
                "Salary": "80000"
              }
            },
            {
              "terms": {
                "Education Level": [
                  "PhD",
                  "Bachelor's"
                ]
              }
            }
          ],
          "should": [
            {
              "term": {
                "Gender": "Male"
              }
            },
            {
              "range": {
                "Age": {
                  "lt": 30
                }
              }
            }
          ],
          "must_not": [
            {
              "term": {
                "Job Title": "Senior Manager"
              }
            }
          ]
        }
      }
    }
  }
}
```
<br>
