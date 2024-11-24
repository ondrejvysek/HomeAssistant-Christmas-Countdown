# Odpočet do Vánoc pro nedočkavé :)

Vánoce za dveřmi, proč si nezablbnout s Home Assistantem a pro nedočkavé 

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
