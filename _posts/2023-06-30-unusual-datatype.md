---
title: Analysis of an Unusual Web Data Format
author: carl_zhou
date: 2023-06-28 10:33:00 +0800
categories: [Programming]
tags: [cryptography, api, web, data structures, python]
pin: false
math: true
mermaid: true
image:
  path: /assets/game-lines.png
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: Site with betting odds
---

I analyzed the API response of a website. For legal reasons, I will not disclose the name of the website or the API URLs. It is a well-known website in the online gambling space.

## The Data Returned

Here is the section of the page which was examined:

![Desktop View](/assets/game-lines.png){: width="700" height="400" }

This HTML was generated from the following response returned from the API call:
![Desktop View](/assets/api-req.png){: width="700" height="400" }

```
F|CL;TK=DZE=;ID=17;IT=#AC#B17#C20836572#D19#E17062955#F19#;OR=0;EX=7,#AC#B17#C20836572#D19#E17062955#F19#@9,BETTINGNEWS17@15,<;ED=1,#AS#B17#Q1#,0@2,#AS#B17#,1;H1=1,#AS#B17#Q1#,0@2,#AS#B17#,1;H2=;NA=,;MF=1;NG=Sports/Ice Hockey Coupon;NS=0;PV=2.0.2220.0/CC;|EV;ID=E1;IT=CC-EV_#AC#B17#C20836572#D19#E17062955#F19#;NA= ;VI=7;CB=??#AN#CA#CU#CZ#IR#KP#ON#RU#SK#SU#SY#US#VE;TB=Hockey,#AS#B17#¬NHL,#AC#B17#C20836572#D48#E972#F10#¬FLA Panthers @ VGS Golden Knights,#ABM#B17#C20836572#D19#E17062955#F19#;EN=1;FI=138529940;BC=20230604010000;SO=0;OI=138529940;|MG;SY=cm;|MA;PD=#AC#B17#C20836572#D19#E17062955#F19#;IT=ACB17C20836572D19E17062955F19CC-MG-AHACB17C20836572D19E17062955F19;NA=Game Betting;LS=1;|MA;PD=#AC#B17#C20836572#D19#E17062955#F19#I99#;IT=ACB17C20836572D19E17062955F19CC-MG-AHACB17C20836572D19E17062955F19I99;NA=Same Game Parlay;BF=1;N2=NEW;FF=HXHX;|MA;PD=#AC#B17#C20836572#D19#E17062955#F19#I1#;IT=ACB17C20836572D19E17062955F19CC-MG-AHACB17C20836572D19E17062955F19I1;NA=Main;|MA;PD=#AC#B17#C20836572#D19#E17062955#F19#I6#;IT=ACB17C20836572D19E17062955F19CC-MG-AHACB17C20836572D19E17062955F19I6;NA=Player;|MA;PD=#AC#B17#C20836572#D19#E17062955#F19#I2#;IT=ACB17C20836572D19E17062955F19CC-MG-AHACB17C20836572D19E17062955F19I2;NA=Score;|MA;PD=#AC#B17#C20836572#D19#E17062955#F19#I3#;IT=ACB17C20836572D19E17062955F19CC-MG-AHACB17C20836572D19E17062955F19I3;NA=1st Period;|MA;PD=#AC#B17#C20836572#D19#E17062955#F19#I4#;IT=ACB17C20836572D19E17062955F19CC-MG-AHACB17C20836572D19E17062955F19I4;NA=1st 10 Minute;|MG;ID=972;NA=Game Lines;PI=972,1530,973;SY=dz;DO=1;|MA;ID=M972;NA= ;FI=138529940;SY=db;PY=da;|PA;ID=PC719999871;NA=Puck Line;|PA;ID=PC719999891;NA=Total;|PA;ID=PC719999906;NA=Money Line;|MA;ID=M972;NA=FLA Panthers;FI=138529940;SY=db;PY=asc;|PA;ID=719999871;FI=138529940;HD=+1.5;HA=+1.5;OD=+1/,;|PA;ID=719999891;FI=138529942;HD=O 5.5;HA=5.5;OD=,.1,-;|PA;ID=719999906;FI=138529943;OD=,-1,.;|MA;ID=M972;NA=VGS Golden Knights;FI=138529940;SY=db;PY=asc;|PA;ID=719999879;FI=138529940;HD=-1.5;HA=-1.5;OD=,1/;|PA;ID=719999897;FI=138529942;HD=U 5.5;HA=5.5;OD=,.1,/;|PA;ID=719999910;FI=138529943;OD=,.1,);|MG;SY=pbj;TL=#PB#AC#B17#FI138529940#;|
```

This does not appear to be a standard data-format such as JSON. At first it appears to be unreadable by eye. 

There appears to be several instances of the \| delimiter. This could represent groups. To attempt to make the data more readable, we split the data into newlines using \| . The result is the following:

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

We can see that key, value pairs are separated by the ; character. The key and value of a pair are separated by the \= character.

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

def _generate_level_obj(input_list):
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
      current_ptr['EV'] = _generate_level_obj(section)
      stack.append(current_ptr)
      current_level = 'EV'
      current_ptr = root['EV']
    elif section[0] == 'CL' and current_level == 'root':
      current_ptr['CL']= _generate_level_obj(section)
    elif section[0] == 'MG' and current_level == 'EV':
      if 'MG' not in current_ptr:
        current_ptr['MG'] = []
      current_ptr['MG'].append(_generate_level_obj(section))
      stack.append(current_ptr)
      current_level = 'MG'
      current_ptr = current_ptr['MG'][-1]
    elif section[0] == 'MA' and current_level == 'MG':
      if 'MA' not in current_ptr:
        current_ptr['MA'] = []
      current_ptr['MA'].append(_generate_level_obj(section))
      stack.append(current_ptr)
      current_level = 'MA'
      current_ptr = current_ptr['MA'][-1]
    elif section[0] == 'MA' and current_level == 'MA':
      current_ptr = stack.pop()
      current_ptr['MA'].append(_generate_level_obj(section))
      stack.append(current_ptr)
      current_ptr = current_ptr['MA'][-1]
    elif section[0] == 'PA' and current_level == 'MA':
      if 'PA' not in current_ptr:
        current_ptr['PA'] = []
      current_ptr['PA'].append(_generate_level_obj(section))
    elif section[0] == 'MG' and current_level == 'MA':
      stack.pop()
      current_ptr = stack.pop()
      current_ptr['MG'].append(_generate_level_obj(section))
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

To maintain the current level in the hierarchy, we utilize a stack. In specific scenarios, we employ the stack to backtrack to MG or MA. However, for all keys following EV, CL, MA, MG, PA, we solely generate key-value pairs at the subsequent level. This process is achieved through the use of the "_generate_level_obj" method, wherein we consider only the first "=" as the delimiter, acknowledging that "=" may also be present within the value.

## An attempt at decrypting the OD value

The OD value may be the key which contains the odds of an event. However it does not match the value displayed in the HTML. For instance, OD=,.1,- displays as -115 and OD=,.1,/ displays as -105.

It is apparent there is an encryption mechanism involved here. 

We will need da larger sample to analyze the ciphertexts. Using this set of HTML for example:

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

There seems to be a discernible pattern when it comes to the OD value. All of the odds exhibit a "*" in the middle.

The odds values are presented in various formats, including US, decimal, and fractions. In the decimal format (e.g., 1.90), the "1" in the middle represents the decimal point, while in fractions format (e.g., 11/10), the "1" denotes the division symbol "/".

For the OD values **-115** and **-105**, the representations are **,.1,-** and **,.1,/,** respectively. This suggests that the odds values are in fractions format, as there are two characters before the "1" rather than just one.

The encryption method employed appears to be a substitution cipher, specifically a Caesar cipher. Decrypting it would be relatively straightforward if we could determine the character mapping.

However, an additional challenge posed by the website is that it changes the character map for every request, making it more difficult to decipher. While some characters can be deduced through frequency analysis (such as "/"), others remain ambiguous without corresponding plaintext.

*The process of reverse engineering the obfuscated client-side JavaScript, which contains the actual decryption code, will not be discussed in detail within this article.*

## Follow-up questions

The question arises as to why this particular website has opted for this format instead of more commonly used formats like JSON or XML. Here are two potential reasons:

**Size** - The chosen format utilized by the website's developers results in a smaller file size compared to JSON output, specifically 2.09 KB versus 2.72 KB. Although the difference in size is negligible on modern computer systems, it is plausible that this format was initially adopted as a legacy format.

**Security** - It is evident that the website's developers aim to deter scraping bots from extracting data from the site. By employing a custom data format, they introduce an obstacle that requires time and effort to decode, although it does not completely prevent scraping attempts.

These reasons suggest that the website's choice of a non-standard format may be driven by considerations of file size efficiency and an intention to impede automated data scraping.