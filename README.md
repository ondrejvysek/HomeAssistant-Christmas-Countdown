# Christmas Countdown for the Impatient :)

With Christmas just around the corner, why not have some fun with Home Assistant and create a countdown for the impatient? 🎄

## Co je potřeba

- [Konfigurace senzoru s configuration.yaml (nemám template.yaml)](#konfigurace-senzoru-s-configurationyaml-nem%C3%A1m-templateyaml)
- [Konfigurace senzoru s template.yaml](#konfigurace-senzoru-s-templateyaml)
- [Tvorba karty pro zobrazení](#tvorba-karty-pro-zobrazen%C3%AD)

---

## Konfigurace senzoru s configuration.yaml (nemám template.yaml)

Pokud nepoužíváte `template.yaml`, můžete přidat konfiguraci senzoru přímo do `configuration.yaml`. To zdali používáte `template.yaml` ověříte tak, že v `configuration.yaml` není tato řádka `template: !include template.yaml`. Pokud ji tam nemáte, můžete rovnou do `configuration.yaml` vložit:

```
sensor:
  - platform: time_date
    display_options:
      - 'date_time'

template:
  - sensor:
      # New Christmas Countdown sensor
      - name: "Christmas Countdown"
        unique_id: christmas_countdown
        state: >
          {% set date_time = states('sensor.date_time') %}
          {% if date_time and date_time != 'unavailable' %}
            {% set clean_date_time = date_time.replace(',', '') %}
            {% set now = clean_date_time | as_datetime %}
            {% set this_year = now.year %}
            {% set target = now.replace(year=this_year, month=12, day=24, hour=17, minute=0, second=0) %}
            {% if now > target %}
              {% set target = target.replace(year=this_year + 1) %}
            {% endif %}
            {{ ((target - now).total_seconds() // 86400) | int }}
          {% else %}
            unavailable
          {% endif %}
        attributes:
          days: >
            {% set date_time = states('sensor.date_time') %}
            {% if date_time and date_time != 'unavailable' %}
              {% set clean_date_time = date_time.replace(',', '') %}
              {% set now = clean_date_time | as_datetime %}
              {% set this_year = now.year %}
              {% set target = now.replace(year=this_year, month=12, day=24, hour=17, minute=0, second=0) %}
              {% if now > target %}
                {% set target = target.replace(year=this_year + 1) %}
              {% endif %}
              {{ ((target - now).total_seconds() // 86400) | int }}
            {% else %}
              unavailable
            {% endif %}
          hours: >
            {% set date_time = states('sensor.date_time') %}
            {% if date_time and date_time != 'unavailable' %}
              {% set clean_date_time = date_time.replace(',', '') %}
              {% set now = clean_date_time | as_datetime %}
              {% set this_year = now.year %}
              {% set target = now.replace(year=this_year, month=12, day=24, hour=17, minute=0, second=0) %}
              {% if now > target %}
                {% set target = target.replace(year=this_year + 1) %}
              {% endif %}
              {{ (((target - now).total_seconds() % 86400) // 3600) | int }}
            {% else %}
              unavailable
            {% endif %}
          minutes: >
            {% set date_time = states('sensor.date_time') %}
            {% if date_time and date_time != 'unavailable' %}
              {% set clean_date_time = date_time.replace(',', '') %}
              {% set now = clean_date_time | as_datetime %}
              {% set this_year = now.year %}
              {% set target = now.replace(year=this_year, month=12, day=24, hour=17, minute=0, second=0) %}
              {% if now > target %}
                {% set target = target.replace(year=this_year + 1) %}
              {% endif %}
              {{ ((((target - now).total_seconds() % 86400) % 3600) // 60) | int }}
            {% else %}
              unavailable
            {% endif %}
        unit_of_measurement: "days"
```


## Konfigurace senzoru s template.yaml

Pokud používáte `template.yaml`, vložte konfiguraci do `configuration.yaml` a `template.yaml`. To zdali používáte `template.yaml` ověříte tak, že v `configuration.yaml` není tato řádka `template: !include template.yaml`.

### configuration.yaml
```
sensor:
  - platform: time_date
    display_options:
      - 'date_time'
```
### template.yaml
```
- sensor:
  # New Christmas Countdown sensor
  - name: "Christmas Countdown"
    unique_id: christmas_countdown
    state: >
      {% set date_time = states('sensor.date_time') %}
      {% if date_time and date_time != 'unavailable' %}
        {% set clean_date_time = date_time.replace(',', '') %}
        {% set now = clean_date_time | as_datetime %}
        {% set this_year = now.year %}
        {% set target = now.replace(year=this_year, month=12, day=24, hour=17, minute=0, second=0) %}
        {% if now > target %}
          {% set target = target.replace(year=this_year + 1) %}
        {% endif %}
        {{ ((target - now).total_seconds() // 86400) | int }}
      {% else %}
        unavailable
      {% endif %}
    attributes:
      days: >
        {% set date_time = states('sensor.date_time') %}
        {% if date_time and date_time != 'unavailable' %}
          {% set clean_date_time = date_time.replace(',', '') %}
          {% set now = clean_date_time | as_datetime %}
          {% set this_year = now.year %}
          {% set target = now.replace(year=this_year, month=12, day=24, hour=17, minute=0, second=0) %}
          {% if now > target %}
            {% set target = target.replace(year=this_year + 1) %}
          {% endif %}
          {{ ((target - now).total_seconds() // 86400) | int }}
        {% else %}
          unavailable
        {% endif %}
      hours: >
        {% set date_time = states('sensor.date_time') %}
        {% if date_time and date_time != 'unavailable' %}
          {% set clean_date_time = date_time.replace(',', '') %}
          {% set now = clean_date_time | as_datetime %}
          {% set this_year = now.year %}
          {% set target = now.replace(year=this_year, month=12, day=24, hour=17, minute=0, second=0) %}
          {% if now > target %}
            {% set target = target.replace(year=this_year + 1) %}
          {% endif %}
          {{ (((target - now).total_seconds() % 86400) // 3600) | int }}
        {% else %}
          unavailable
        {% endif %}
      minutes: >
        {% set date_time = states('sensor.date_time') %}
        {% if date_time and date_time != 'unavailable' %}
          {% set clean_date_time = date_time.replace(',', '') %}
          {% set now = clean_date_time | as_datetime %}
          {% set this_year = now.year %}
          {% set target = now.replace(year=this_year, month=12, day=24, hour=17, minute=0, second=0) %}
          {% if now > target %}
            {% set target = target.replace(year=this_year + 1) %}
          {% endif %}
          {{ ((((target - now).total_seconds() % 86400) % 3600) // 60) | int }}
        {% else %}
          unavailable
        {% endif %}
    unit_of_measurement: "days"
```
## Tvorba karty pro zobrazení

### Jednoduchá markdown karta
![image](https://github.com/user-attachments/assets/05d9efa6-6197-430b-bb1c-f7cb460559f4)

```
type: markdown
content: |2-

    **{{ state_attr('sensor.christmas_countdown', 'days') }}** dnů, **{{ state_attr('sensor.christmas_countdown', 'hours') }}** hodin, **{{ state_attr('sensor.christmas_countdown', 'minutes') }}** minut
title: Odpočet do Vánoc
```

### Jednoduchá karta entity
![image](https://github.com/user-attachments/assets/febfa6ea-9c97-4bd4-b968-76444dc49a43)

```
type: entity
entity: sensor.christmas_countdown
icon: mdi:pine-tree-fire
unit: Dnů
name: Odpočet do vánoc
```

### Karta s obrázkem
![image](https://github.com/user-attachments/assets/7530a1ea-f3f8-49f4-aabc-ccfceb63ae6a)

Ve vlastnostech karty nahrajte obrázek dle chuti pro světlé a tmavé téma, můžete použít [např tento](https://github.com/ondrejvysek/HomeAssistant-Christmas-Countdown/blob/main/green-christmas-tree.png)     

```
type: picture-elements
elements:
  - type: state-label
    style:
      left: 50%
      top: 50%
      color: black
      font-size: 100px
      background-color: white
      border-radius: 50%
      width: 200px
      height: 200px
      display: flex
      align-items: center
      justify-content: center
      text-align: center
      transform: translate(-50%, -50%)
      box-shadow: 0px 4px 6px rgba(0, 0, 0, 0.2)
    entity: sensor.christmas_countdown
    attribute: days
image: /api/image/serve/ef1833840d9b3a7ef46e7c13773b54d1/512x512
dark_mode_image: /api/image/serve/ef1833840d9b3a7ef46e7c13773b54d1/512x512
```
