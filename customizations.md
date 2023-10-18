A list of customizations to the base CollectionBuilder files that we've made:

### Update metadata layout for external links

Add ability for a metadata field marked as "external_link" to take multiple values, separated by a semicolon (`;`). Render multiple external links as an unordered list. Recommend to add custom CSS to change the list style of the list to none. `ul.external-link-list { list-style: none; }`

```
vvvvvvvvvvvvvvvvvvv Original vvvvvvvvvvvvvvvvvvv
  if (item[{{ f.field | jsonify }}]) { fields += '<dt class="field">{{ f.display_name }}:</dt> <dd class="field-value">{% if f.external_link == "true" %}<a href="' + item[{{ f.field | jsonify }}] + '" target="_blank" rel="noopener">' + item[{{ f.field | jsonify }}] + '</a>{% else %}' + item[{{ f.field | jsonify }}] + '{% endif %}</dd>'; }
  
vvvvvvvvvvvvvvvvvvv Custom vvvvvvvvvvvvvvvvvvv
  if (item[{{ f.field | jsonify }}]) {
      const field = {{ f | jsonify }};
      const value = item[field.field];

      let valueElAsStr = `${value}`;
      if (field.external_link) {
          valueElAsStr = 
              '<ul class="external-link-list">' +
              value
                  .split(';')
                  .map((link) => (
                      `<li><a href=${link} target="_blank" rel="noopener">${link}</a></li>`
                  ))
                  .join('') +
              '</ul>'
      }

      fields += `<dt class="field">{{ f.display_name }}:</dt><dd class="field-value">${valueElAsStr}</dd>`;
  }
```
