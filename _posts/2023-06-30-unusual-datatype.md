---
title: Analyzing an Unusual Web Data Format
author: carl_zhou
date: 2023-06-28 10:33:00 +0800
categories: [Tech]
tags: [cryptography, api, web, data structures, python]
pin: false
math: true
mermaid: true
image:
  path: /assets/game-lines.png
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: Site with betting odds - Florida Panthers vs Vegas Golden Knights
---

I analyzed the API response of a website. For legal reasons, I will not disclose the name of the website or the API URLs. It is a well-known website in the online gambling space.

## The Data Returned

Here is the section of the page which was examined:

![Desktop View](/assets/game-lines.png){: width="700" height="400" }

This HTML was generated from the following response returned from the API call:

```
F|CL;TK=DZE=;ID=17;IT=#AC#B17#C20836572#D19#E17062955#F19#;OR=0;EX=7,#AC#B17#C20836572#D19#E17062955#F19#@9,BETTINGNEWS17@15,<;ED=1,#AS#B17#Q1#,0@2,#AS#B17#,1;H1=1,#AS#B17#Q1#,0@2,#AS#B17#,1;H2=;NA=,;MF=1;NG=Sports/Ice Hockey Coupon;NS=0;PV=2.0.2220.0/CC;|EV;ID=E1;IT=CC-EV_#AC#B17#C20836572#D19#E17062955#F19#;NA= ;VI=7;CB=??#AN#CA#CU#CZ#IR#KP#ON#RU#SK#SU#SY#US#VE;TB=Hockey,#AS#B17#¬NHL,#AC#B17#C20836572#D48#E972#F10#¬FLA Panthers @ VGS Golden Knights,#ABM#B17#C20836572#D19#E17062955#F19#;EN=1;FI=138529940;BC=20230604010000;SO=0;OI=138529940;|MG;SY=cm;|MA;PD=#AC#B17#C20836572#D19#E17062955#F19#;IT=ACB17C20836572D19E17062955F19CC-MG-AHACB17C20836572D19E17062955F19;NA=Game Betting;LS=1;|MA;PD=#AC#B17#C20836572#D19#E17062955#F19#I99#;IT=ACB17C20836572D19E17062955F19CC-MG-AHACB17C20836572D19E17062955F19I99;NA=Same Game Parlay;BF=1;N2=NEW;FF=HXHX;|MA;PD=#AC#B17#C20836572#D19#E17062955#F19#I1#;IT=ACB17C20836572D19E17062955F19CC-MG-AHACB17C20836572D19E17062955F19I1;NA=Main;|MA;PD=#AC#B17#C20836572#D19#E17062955#F19#I6#;IT=ACB17C20836572D19E17062955F19CC-MG-AHACB17C20836572D19E17062955F19I6;NA=Player;|MA;PD=#AC#B17#C20836572#D19#E17062955#F19#I2#;IT=ACB17C20836572D19E17062955F19CC-MG-AHACB17C20836572D19E17062955F19I2;NA=Score;|MA;PD=#AC#B17#C20836572#D19#E17062955#F19#I3#;IT=ACB17C20836572D19E17062955F19CC-MG-AHACB17C20836572D19E17062955F19I3;NA=1st Period;|MA;PD=#AC#B17#C20836572#D19#E17062955#F19#I4#;IT=ACB17C20836572D19E17062955F19CC-MG-AHACB17C20836572D19E17062955F19I4;NA=1st 10 Minute;|MG;ID=972;NA=Game Lines;PI=972,1530,973;SY=dz;DO=1;|MA;ID=M972;NA= ;FI=138529940;SY=db;PY=da;|PA;ID=PC719999871;NA=Puck Line;|PA;ID=PC719999891;NA=Total;|PA;ID=PC719999906;NA=Money Line;|MA;ID=M972;NA=FLA Panthers;FI=138529940;SY=db;PY=asc;|PA;ID=719999871;FI=138529940;HD=+1.5;HA=+1.5;OD=+1/,;|PA;ID=719999891;FI=138529942;HD=O 5.5;HA=5.5;OD=,.1,-;|PA;ID=719999906;FI=138529943;OD=,-1,.;|MA;ID=M972;NA=VGS Golden Knights;FI=138529940;SY=db;PY=asc;|PA;ID=719999879;FI=138529940;HD=-1.5;HA=-1.5;OD=,1/;|PA;ID=719999897;FI=138529942;HD=U 5.5;HA=5.5;OD=,.1,/;|PA;ID=719999910;FI=138529943;OD=,.1,);|MG;SY=pbj;TL=#PB#AC#B17#FI138529940#;|
```

This does not appear to be a standard data-format such as JSON. At first it appears to be unreadable by eye. 

It appears that groups of data are separated by the \| character and key, value pairs are separated by the ; character.

To make the data more readable, new lines can be split using \| . The result is the following:

```
F

CL;TK=DZE=;ID=17;IT=#AC#B17#C20836572#D19#E17062955#F19#;OR=0;EX=7,#AC#B17#C20836572#D19#E17062955#F19#@9,BETTINGNEWS17@15,<;ED=1,#AS#B17#Q1#,0@2,#AS#B17#,1;H1=1,#AS#B17#Q1#,0@2,#AS#B17#,1;H2=;NA=,;MF=1;NG=Sports/Ice Hockey Coupon;NS=0;PV=2.0.2220.0/CC;

EV;ID=E1;IT=CC-EV_#AC#B17#C20836572#D19#E17062955#F19#;NA= ;VI=7;CB=??#AN#CA#CU#CZ#IR#KP#ON#RU#SK#SU#SY#US#VE;TB=Hockey,#AS#B17#¬NHL,#AC#B17#C20836572#D48#E972#F10#¬FLA Panthers @ VGS Golden Knights,#ABM#B17#C20836572#D19#E17062955#F19#;EN=1;FI=138529940;BC=20230604010000;SO=0;OI=138529940;

MG;SY=cm;

MA;PD=#AC#B17#C20836572#D19#E17062955#F19#;IT=ACB17C20836572D19E17062955F19CC-MG-AHACB17C20836572D19E17062955F19;NA=Game Betting;LS=1;

MA;PD=#AC#B17#C20836572#D19#E17062955#F19#I6#;IT=ACB17C20836572D19E17062955F19CC-MG-AHACB17C20836572D19E17062955F19I6;NA=Player;

MA;PD=#AC#B17#C20836572#D19#E17062955#F19#I2#;IT=ACB17C20836572D19E17062955F19CC-MG-AHACB17C20836572D19E17062955F19I2;NA=Score;

MA;PD=#AC#B17#C20836572#D19#E17062955#F19#I3#;IT=ACB17C20836572D19E17062955F19CC-MG-AHACB17C20836572D19E17062955F19I3;NA=1st Period;

MA;PD=#AC#B17#C20836572#D19#E17062955#F19#I4#;IT=ACB17C20836572D19E17062955F19CC-MG-AHACB17C20836572D19E17062955F19I4;NA=1st 10 Minute;

MG;ID=972;NA=Game Lines;PI=972,1530,973;SY=dz;DO=1;

MA;ID=M972;NA= ;FI=138529940;SY=db;PY=da;

PA;ID=PC719999871;NA=Puck Line;

PA;ID=PC719999891;NA=Total;

PA;ID=PC719999906;NA=Money Line;

MA;ID=M972;NA=FLA Panthers;FI=138529940;SY=db;PY=asc;

PA;ID=719999871;FI=138529940;HD=+1.5;HA=+1.5;OD=+1/,;

PA;ID=719999891;FI=138529942;HD=O 5.5;HA=5.5;OD=,.1,-;

PA;ID=719999906;FI=138529943;OD=,-1,.;

MA;ID=M972;NA=VGS Golden Knights;FI=138529940;SY=db;PY=asc;

PA;ID=719999879;FI=138529940;HD=-1.5;HA=-1.5;OD=,1/;

PA;ID=719999897;FI=138529942;HD=U 5.5;HA=5.5;OD=,.1,/;

PA;ID=719999910;FI=138529943;OD=,.1,);

MG;SY=pbj;TL=#PB#AC#B17#FI138529940#;
```

Based on this result, a set of data always begins with one of the following keys: F, CL, EV, MG, MA,PA. Since F does not have any data after it, it represents a marker as the start of the data.

## Converting the custom datatype to JSON

The first element in the ; separator has repeating elements in MG, MA, and PA. The following hierarchy can be derived:

CL

EV -> MG -> MA-> PA

Based on the above hierarchy, the output would look something like this:

```json
{
  CL: {
    TK: DZE=,
    ID: 17,
    IT:
  },
  EV: {
    ID:E1,
    IT: CC-EV_#AC#B17#C20836572#D19#E17062955#F19#,
    NA:'',
    H2:'',
    MG: [
      {
      SY: cm,
      MA: [
        {
          PD: #AC#B17#C20836572#D19#E17062955#F19#,
          IT: ACB17C20836572D19E17062955F19CC-MG-AHACB17C20836572D19E17062955F19,
          NA: Game Betting,
          LS:1
        },
        {
          PD: #AC#B17#C20836572#D19#E17062955#F19#I99#,
          IT: ACB17C20836572D19E17062955F19CC-MG-AHACB17C20836572D19E17062955F19I99,
          NA: Same Game Parlay,
          BF:1
        }
      ]
    },
      {
        ID: 972
        MA: [
          {
            PA: [
              {
                
              }
            ]
          }
        ]
      }
    ]
  }
}
```

Based on the target output, we can use this Python code to convert the input to JSON format as output:

```python
import json

def generate_obj(input_list):
  res = {}
  for item in input_list[1:]:
    if '=' in item:
      key,value = item.split('=',1)
      res[key] = value
  return res

def generate_json(input):
  sections = list(map(lambda x: x.split(';'), input.split('|')))
  root = {}
  current_level = 'root'
  current_ptr = root
  stack = []
  for section in sections:
    if section[0] == 'EV' and current_level == 'root':
      current_ptr['EV'] = generate_obj(section)
      stack.append(current_ptr)
      current_level = 'EV'
      current_ptr = root['EV']
    elif section[0] == 'CL' and current_level == 'root':
      current_ptr['CL']= generate_obj(section)
    elif section[0] == 'MG' and current_level == 'EV':
      if 'MG' not in current_ptr:
        current_ptr['MG'] = []
      current_ptr['MG'].append(generate_obj(section))
      stack.append(current_ptr)
      current_level = 'MG'
      current_ptr = current_ptr['MG'][-1]
    elif section[0] == 'MA' and current_level == 'MG':
      if 'MA' not in current_ptr:
        current_ptr['MA'] = []
      current_ptr['MA'].append(generate_obj(section))
      stack.append(current_ptr)
      current_level = 'MA'
      current_ptr = current_ptr['MA'][-1]
    elif section[0] == 'MA' and current_level == 'MA':
      current_ptr = stack.pop()
      current_ptr['MA'].append(generate_obj(section))
      stack.append(current_ptr)
      current_ptr = current_ptr['MA'][-1]
    elif section[0] == 'PA' and current_level == 'MA':
      if 'PA' not in current_ptr:
        current_ptr['PA'] = []
      current_ptr['PA'].append(generate_obj(section))
    elif section[0] == 'MG' and current_level == 'MA':
      stack.pop()
      current_ptr = stack.pop()
      current_ptr['MG'].append(generate_obj(section))
      stack.append(current_ptr)
      current_ptr=current_ptr['MG'][-1]
      current_level = 'MG'

  return json.dumps(root)
```

Calling our method "generate_json" returns the following:

```python
data = "F|CL;TK=DZE=;ID=17;I..."
print(generate_json(data))
```

```json
{
  "CL": {
    "TK": "DZE=",
    "ID": "17",
    "IT": "#AC#B17#C20836572#D19#E17062955#F19#",
    "OR": "0",
    "EX": "7,#AC#B17#C20836572#D19#E17062955#F19#@9,BETTINGNEWS17@15,<",
    "ED": "1,#AS#B17#Q1#,0@2,#AS#B17#,1",
    "H1": "1,#AS#B17#Q1#,0@2,#AS#B17#,1",
    "H2": "",
    "NA": ",",
    "MF": "1",
    "NG": "Sports/Ice Hockey Coupon",
    "NS": "0",
    "PV": "2.0.2220.0/CC"
  },
  "EV": {
    "ID": "E1",
    "IT": "CC-EV_#AC#B17#C20836572#D19#E17062955#F19#",
    "NA": " ",
    "VI": "7",
    "CB": "??#AN#CA#CU#CZ#IR#KP#ON#RU#SK#SU#SY#US#VE",
    "TB": "Hockey,#AS#B17#¬NHL,#AC#B17#C20836572#D48#E972#F10#¬FLA Panthers @ VGS Golden Knights,#ABM#B17#C20836572#D19#E17062955#F19#",
    "EN": "1",
    "FI": "138529940",
    "BC": "20230604010000",
    "SO": "0",
    "OI": "138529940",
    "MG": [
      {
        "SY": "cm",
        "MA": [
          {
            "PD": "#AC#B17#C20836572#D19#E17062955#F19#",
            "IT": "ACB17C20836572D19E17062955F19CC-MG-AHACB17C20836572D19E17062955F19",
            "NA": "Game Betting",
            "LS": "1"
          },
          {
            "PD": "#AC#B17#C20836572#D19#E17062955#F19#I99#",
            "IT": "ACB17C20836572D19E17062955F19CC-MG-AHACB17C20836572D19E17062955F19I99",
            "NA": "Same Game Parlay",
            "BF": "1",
            "N2": "NEW",
            "FF": "HXHX"
          },
          {
            "PD": "#AC#B17#C20836572#D19#E17062955#F19#I1#",
            "IT": "ACB17C20836572D19E17062955F19CC-MG-AHACB17C20836572D19E17062955F19I1",
            "NA": "Main"
          },
          {
            "PD": "#AC#B17#C20836572#D19#E17062955#F19#I6#",
            "IT": "ACB17C20836572D19E17062955F19CC-MG-AHACB17C20836572D19E17062955F19I6",
            "NA": "Player"
          },
          {
            "PD": "#AC#B17#C20836572#D19#E17062955#F19#I2#",
            "IT": "ACB17C20836572D19E17062955F19CC-MG-AHACB17C20836572D19E17062955F19I2",
            "NA": "Score"
          },
          {
            "PD": "#AC#B17#C20836572#D19#E17062955#F19#I3#",
            "IT": "ACB17C20836572D19E17062955F19CC-MG-AHACB17C20836572D19E17062955F19I3",
            "NA": "1st Period"
          },
          {
            "PD": "#AC#B17#C20836572#D19#E17062955#F19#I4#",
            "IT": "ACB17C20836572D19E17062955F19CC-MG-AHACB17C20836572D19E17062955F19I4",
            "NA": "1st 10 Minute"
          }
        ]
      },
      {
        "ID": "972",
        "NA": "Game Lines",
        "PI": "972,1530,973",
        "SY": "dz",
        "DO": "1",
        "MA": [
          {
            "ID": "M972",
            "NA": " ",
            "FI": "138529940",
            "SY": "db",
            "PY": "da",
            "PA": [
              {
                "ID": "PC719999871",
                "NA": "Puck Line"
              },
              {
                "ID": "PC719999891",
                "NA": "Total"
              },
              {
                "ID": "PC719999906",
                "NA": "Money Line"
              }
            ]
          },
          {
            "ID": "M972",
            "NA": "FLA Panthers",
            "FI": "138529940",
            "SY": "db",
            "PY": "asc",
            "PA": [
              {
                "ID": "719999871",
                "FI": "138529940",
                "HD": "+1.5",
                "HA": "+1.5",
                "OD": "+1/,"
              },
              {
                "ID": "719999891",
                "FI": "138529942",
                "HD": "O 5.5",
                "HA": "5.5",
                "OD": ",.1,-"
              },
              {
                "ID": "719999906",
                "FI": "138529943",
                "OD": ",-1,."
              }
            ]
          },
          {
            "ID": "M972",
            "NA": "VGS Golden Knights",
            "FI": "138529940",
            "SY": "db",
            "PY": "asc",
            "PA": [
              {
                "ID": "719999879",
                "FI": "138529940",
                "HD": "-1.5",
                "HA": "-1.5",
                "OD": ",1/"
              },
              {
                "ID": "719999897",
                "FI": "138529942",
                "HD": "U 5.5",
                "HA": "5.5",
                "OD": ",.1,/"
              },
              {
                "ID": "719999910",
                "FI": "138529943",
                "OD": ",.1,)"
              }
            ]
          }
        ]
      },
      {
        "SY": "pbj",
        "TL": "#PB#AC#B17#FI138529940#"
      }
    ]
  }
}
```

**A brief explanation of the code** 

We use a stack to keep track of the current level in the hierarchy. Then in certain cases we backtrack to MG or MA with the help of the stack. For all other keys after EV,CL,MA,MG,PA, we only create key value pairs at the next level. This is accomplished with generate_obj, where we only consider the first "=" to be the delimiter, given that "=" can also exist in the value.

## An attempt at decrypting the OD value

It appears that the OD value is the key which contains the odds of an event. The OD value, does not appear to match the value displayed in the HTML. For instance, OD=,.1,- displays as -115 and OD=,.1,/ displays as -105.

On the surface, there appears to be no obvious correlation between the two. They are of different character lengths. 

Perhaps we need a larger sample. Using this set of HTML for example:

![Desktop View](/assets/correct-score.png){: width="700" height="400" }

This was generated by the API call which returned the following data:
```
MG;ID=1553;NA=Correct Score;SY=dz;DO=1;
MA;ID=RB4_21553;NA= ;FI=138531656;SY=dc;PY=de;
PA;ID=PC720150957;NA= 1-0;
PA;ID=PC720150966;NA= 2-0;
PA;ID=PC720151048;NA= 3-0;
PA;ID=PC720151050;NA= 4-0;
PA;ID=PC720151052;NA= 5-0;
PA;ID=PC720151054;NA= 6-0;
PA;ID=PC720151056;NA= 2-1;
PA;ID=PC720151057;NA= 3-1;
PA;ID=PC720151059;NA= 4-1;
PA;ID=PC720151061;NA= 5-1;
PA;ID=PC720151063;NA= 6-1;
PA;ID=PC720151064;NA= 3-2;
PA;ID=PC720151066;NA= 4-2;
PA;ID=PC720151068;NA= 5-2;
PA;ID=PC720151070;NA= 6-2;
PA;ID=PC720151071;NA= 4-3;
PA;ID=PC720151073;NA= 5-3;
PA;ID=PC720151075;NA= 6-3;
PA;ID=PC720151077;NA= 5-4;
PA;ID=PC720151078;NA= 6-4;
PA;ID=PC720151080;NA= 6-5;
MA;ID=M11553;NA=FLA Panthers;FI=138531656;SY=dg;PY=_d;
PA;ID=720150957;OD=05*4;
PA;ID=720150966;OD=15*4;
PA;ID=720151048;OD=60*4;
PA;ID=720151050;OD=15*4;
PA;ID=720151052;OD=30*4;
PA;ID=720151054;OD=470*4;
PA;ID=720151056;OD=41*4;
PA;ID=720151057;OD=43*4;
PA;ID=720151059;OD=4=*4;
PA;ID=720151061;OD=60*4;
PA;ID=720151063;OD=30*4;
PA;ID=720151064;OD=<*4;
PA;ID=720151066;OD=43*4;
PA;ID=720151068;OD=75*4;
PA;ID=720151070;OD=15*4;
PA;ID=720151071;OD=47*4;
PA;ID=720151073;OD=70*4;
PA;ID=720151075;OD=15*4;
PA;ID=720151077;OD=70*4;
PA;ID=720151078;OD=05*4;
PA;ID=720151080;OD=05*4;
```
From this section, we can see that each ID is associated with ID=PC{ID} under PA. This could mean that each ID which begins with PC means (Partial component).

In terms of the OD value, there is a pattern here. All of the odds have * in the middle. 

Odds values exist in formats -  US, decimal, fractions. The "1" in the middle could either be the "." in decimal odds format (1.90) or the "/" in fractions format (11/10). 

The OD values for **-115** and **-105** are **,.1,-** and **,.1,/** respectively. This would suggest the odds values are fractions, because of 2 characters before the "1", instead of 1. 

The encryption method appears to be a substitution (caesar) cipher. This could be easily decrypted, given that we can determine the character mapping. 

One additional mechanism the website implements here which works against us here is that the website changes the character map for every request. While we can figure out a few characters using frequency counts (such as /), some of the other characters can be ambiguous, without any corresponding plaintext. 

*Reverse engineering the obfuscated client-side JavaScript containing the actual decryption code will not be discussed in this article.*

## Follow-up questions

The question we have is, why a developer would choose to use such a format, over a standard format such as JSON or XML?

Here are two possible reasons:

1. Size - The format used by the website's developers generates a smaller size than the JSON output - 2.09 KB vs 2.72 KB. 
  The size difference here is negligible on modern computer systems. However, it is possible this format was used as a legacy format.
2. Security - It is obvious the website's developers does not want bots to scrape the website. Using a custom data format will not stop scraping attempts, but it will be an obstacle which requires time and effort to decode.