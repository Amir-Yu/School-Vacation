# Israel School-Vacation with HomeAssistant Sensor Custom Component
## Get Israel School Vacation in HomeAssistant

This sensor checks whether today is a day off from school or not.
The sensor is divided into two types, elementary school and high school.
In Israel, the vacation begins on 22.06 in high school, while in elementary school the vacation begins on 01.07. 
Therefore there is a separation that allows the creation of separate automations for a school type.

It's very useful if you set a input_bolean in HA cause you can make automation that set if is vacation or not..
example :
```python
- id: Set_School_Mode_Off
  alias: Set School Mode Off
  trigger: 
  - platform: state
    entity_id: sensor.school_is_elementary_vacation_today
    to: 'True'
  condition: []
  action:
  - data:
      entity_id: input_boolean.school_auto
    service: input_boolean.turn_off
 ```
 ## Guide How to use it
       
### Requirements
 * First need to create folder "school_holidays" in your HomeAssistant config /custom_components folder
* Copy python file "sensor.py , manifest.json , __init__.py " to the HA config /custom_components/school_holidays/ folder.
* Now you need to add those lines in HA sensor.yaml (if you separates your configs file)  /   :
 ```python
 - platform: school_holidays
   elementary_school: True
   high_school: True
   resources:
   - is_high_vacation_today
   - is_elementary_vacation_today
   - is_high_vacation_nextday
   - is_elementary_vacation_nextday
   - summary_today
  ```
  And if you want to use it with input_boolean here is the example to add to input_boolean.yaml file:
  ```python
  school_auto:
    name: School Mode
    icon: mdi:school
  ```
  Or if all yours HA configuration in configurtion.yaml file , use this example:
  ```python
  sensor:
    - platform: school_holidays
      elementary_school: True
      high_school: True
      resources:
      - is_high_vacation
      - is_elementary_vacation
      - is_high_vacation_nextday
      - is_elementary_vacation_nextday
      - summary_today
  
  input_boolean:
    school_auto:
      name: School Mode
      icon: mdi:school
      
  ```
  ### Entity Requirment
  
  elementary_school: get True when vacation and False if not
  high_school: get True when vacation and False if not
  
  ### Sensor Views Options :
 * in group.yaml:
 ```python
  school:
  name: "מצב לימודים"
  view: no
  entities:
    - sensor.school_summary
    - input_boolean.school_auto 
 ```
 
 * Or in ui-lovelace.yaml :
 
 ```python
  - type: entities
        title: "בית ספר"
        show_header_toggle: false
        entities:
          - sensor.school_summary
          - input_boolean.school_auto  
 ```
 * All sensors icon already set , but you can always customize them..
 
 # Good Luck !
