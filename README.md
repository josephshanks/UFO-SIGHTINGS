![NLP Header](https://github.com/boogiedev/UFO-SIGHTINGS/blob/master/media/ufo-header.png)

<p align="center">
  <img src="https://img.shields.io/badge/Maintained%3F-IN PROG-blue?style=flat-square"></img>
  <img src="https://img.shields.io/github/commit-activity/m/boogiedev/UFO-SIGHTINGS?style=flat-square"></img>
</p>


## Team

[Tyler Woods](https://github.com/tylerjwoods)  | [Joseph Shanks](https://github.com/josephshanks) | [Wesley Nguyen](https://github.com/boogiedev)
---|---|---|

 
## Table of Contents

- [Basic Overview](#basic-overview)
  - [Context](#context)
  - [Goal](#goal)
- [Exploring Data](#exploring-data)
  - [Initial Intake](#initial-intake)
  - [Feature Engineering](#feature-engineering)
- [Language Processing](#language-processing)
  - [Tokenizing](#tokenizing)
- [Future Considerations](#future-considerations)
- [License](#license)
- [Credits](#credits)
- [Thanks](#thanks)

## Basic Overview

### Context

<img align="right" src="https://i.pinimg.com/236x/32/47/16/324716a77ab7183025a1ad46786de375--x-files-funny-love-puns.jpg">

It's a bird... it's a plane...it's... a U.F.O. sighting? Over the course of human history U.F.O. sightings seem to be commonplace; commonly described as "flying saucers", strange lights and objects or straight up "Aliens". There are a lot of unknowns that surround the idea of unidentified flying objects, but one thing that is known, are that people are continually fascinated by them. Today, we are looking at reported U.F.O. sightings from ![THE NATIONAL UFO REPORTING CENTER](http://www.nuforc.org/). These are anonymous reports from various people all over the U.S. and sometimes even internationally.

### Goal

Using Natural Language Processing, we are hoping to parse through these sighting reports and explore possible commonalities, insights, and sentiment about these suspicious objects. By doing this, we are hoping to gain a more concrete truth of whether these sightings are figments of people's imaginations, or that there might be actually be an alien overlord visiting us from time to time.

> If reports from Alabama mention that this mysterious "object" has the shape of a rectangle, while reports from other states express the same thing, is it possible to make a connection here?  


## Exploring Data

SOURCE             | TIMEFRAME 
:-------------------------:|:-------------------------:|
![NUFORC](http://www.nuforc.org/)  | MAY 10th 2017  

The specific data that we are focusing on today are U.F.O. reports from May 10th, 2017, with a total of 99 sightings. Below is a preview of the format that the data comes in from the website.

<p align="center">
  <img src="https://raw.githubusercontent.com/boogiedev/UFO-SIGHTINGS/master/media/dataexcerpt.PNG"></img>
</p>



### Initial Intake

Here is a detailed description of the intake data:
- `_id/$oid`: city this user signed up in phone: primary device for this user
- `url`: date of account registration; in the form `YYYYMMDD`
- `html`: the last time this user completed a trip; in the form `YYYYMMDD`
- `time`: the average distance (in miles) per trip taken in the first 30 days after signup

<p align="center">
  <img src="https://raw.githubusercontent.com/boogiedev/UFO-SIGHTINGS/master/media/dfbefore.PNG"></img>
</p>


### Feature Engineering / Data Cleaning

Looking at this data, we noticed that there were a couple things that could be cleaned and changed. 

```
def clean_data(data:pd.DataFrame) -> pd.DataFrame:
    """Cleaner for UFO DataFrame"""
    # Copy data to avoid collision
    df_copy = data.copy()
    
    
    # Rename ID Column for Clarity
    df_copy.columns.values[0] = 'ID'
    
    # Convert Time to DateTime Object
    df_copy['time'] = pd.to_datetime(df_copy['time'])
    
    # Parse data from HTML Column
    df_copy['state'] = None
    df_copy['content'] = None
    df_copy['shape'] = None
    for i in range(len(df_copy)):
        soup = BeautifulSoup(df_copy['html'][i], 'html.parser')
        meta_data = soup.find_all('tbody')[0].find_all('tr')[0]
        s = meta_data.get_text('|', strip=True).split("|")
        # store data into a dictionary
        s_dict = {x.partition(":")[0]:x.partition(":")[-1] for x in s}
        state = s_dict['Location'][-2:]
        df_copy.loc[i, 'state'] = state
        entry = soup.find_all('tbody')[0].find_all('tr')[1]
        df_copy.loc[i, 'content'] = entry.get_text(strip=True)
        duration = s_dict['Duration']
        df_copy.loc[i, 'duration'] = duration
        shape = s_dict['Shape']
        df_copy.loc[i, 'shape'] = shape
    
    return df_copy
```

We ended up 


---
## Language Processing

### Tokenizing

Fill

## Future Considerations

Using NaieveBayes to test comminalities of words used to derive if these occurences are related.


Do the U.F.O. sightings have a similar distribution of reports from states?


## License
[MIT ©](https://choosealicense.com/licenses/mit/)

## Credits

Fill

## Thanks

Fill
