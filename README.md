## Progress

I had to learn quite a bit of Vue as I was more familiar
with other frameworks before but with new knowledge I was 
able to provide new functionality to the dashboard-sla project.

(Oct 31) First, I got the project running and replaced all Vue 2
dependencies and versions up to 3 and subsequently refactored
based on that.

(Nov 1) Then I decided to break up the one table.vue into table.vue 
and checkboxes.vue so that I could start creating single-file 
components. At the end of the day, I had a working version
using broken up components.

(Nov 2) The first bit of new functionality was pagination. I
kept it to 100 rows per page and added a number input to 
let the user go between pages. I used a calculation of the max 
page to determine when to disable these elements.

(Nov 3) I fully fleshed out the max page implementation. Then I
added dynamic row coloration for several statuses. I also created
buttons for pagination for ease of use.

(Nov 5) I added in a search bar to allow quick filtering based on
any of the properties of a data element (ie Product, Cores). I also
added styling and custom fonts to prettify the application.

## Decisions

When breaking into single file components, I decided to group functionalities
together into different components. Thus I made a table.vue and checkboxes.vue.
Subsequently I decided to group pagination into pageselector.vue and
the search functionality into searchbar.vue. My parent component is dashboard.vue.
I decided to have the data live in dashboard.vue so I could easily manipulate it
based on each of the components before effectively sending it to table.vue to
be formatted and presented on the site itself.

I decided to more or less leave the checkboxes and table alone since they
were already working well.

The page selector is also a single functionality component that only interacts with
the page number and increments or decrements it based on either the left/right buttons
or the number text input to simplify components. Keeping it to a max page limit 
is done in dashboard.vue as to further separate functionality, but this limit is 
communicated to the page selector. Additionally, when any searching or checkbox hiding
is done, the page is reset to 1.

The search bar is the newer functionality and I decided to have it filter the list
based on the attributes of each data element because the user will want to see similar
data as they go through the table and display it accordingly.

## Local Setup

```sh
npm install
```

### Compile and Hot-Reload for Development

```sh
npm run dev
```

### Compile and Minify for Production

```sh
npm run build
```


## Bonus Features

The bonus feature I implemented was the search bar. It filters the list based
on the data attributes such as lithography, product, threads, etc. The user must
type their single filter in and hit the "Enter" button to see it. Clearing the textbox
will reset the filtering.

To use the search functionality, the user enters in the attribute followed by what they
want to filter. To keep it open to more data attributes or values, it is case sensitive.
And example would be if the user enters "Lithography: <specified lithography value>" 
or something similar for other attributes i.e. threads, base freq. Then when they hit
enter, it will show only elements with those attributes. To reset it, the user can
clear the textbox of its text and it will undo any filtering.

If the user doesn't specify any attribute to filter, it will assume that it's filtering
on product.
