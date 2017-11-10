## gh_language_graph

# Functional and stylistic modifications made to GitHub's Developer's Guide, Rendering Data as Graphs

Bulk of code came from: @https://developer.github.com/v3/guides/rendering-data-as-graphs/

## Modifications made in the server.rb file and in the view.

### In server.rb:
  Restrict display of Coffee Script (#of bytes was around 800) with the following funciton:

  ```
  language_byte_count = language_byte_count.select do |hash|
     hash[:count] < 50 || hash[:count] > 10000
   end
   ```

### In lang_freq.erb:
  Alter styles and html to make both graphics display side-by-side
  Change color scheme

  In first script (rendering the bar graph), make sizing mods

  ```
  var barWidth = 50;
  var width = (barWidth + 12) * data.length;
  var height = 380;
  ```

  In second script (rendering the Treemap):
    - Modify size of the Treemap, down to 400 x 400
      - In original:

        ```
        drawTreemap(5000, 2000, ...

        ```

    - The text labels exist as innerText for each div with class="cell". They were crammed against box edge and not easily readable, so I experimented with adding padding and margins to each 'cell' div.

    - This created a more interesting visual, but the JavaScript label was getting covered up. To remedy this, I used the **following set of JS** to modify the padding and margin on the HTML and JS boxes, respectively.


    ```
        let htmlName = language_bytes[0].elements[5].name
        let jsName = language_bytes[0].elements[4].name
        console.log(jsName)

        let cellDivs = document.getElementsByClassName('cell')

        function readCellDivs (textEl) {
          for (i = 0; i < cellDivs.length; i++){
                if (cellDivs[i].innerText === textEl){
                    return cellDivs[i]
                }
            }
        }
        let htmlBox = readCellDivs(htmlName)
        htmlBoxCurrentStyle = htmlBox.getAttribute('style')
        htmlBox.setAttribute('style', htmlBoxCurrentStyle + "padding-bottom: 0px")

        let jsBox = readCellDivs(jsName)
        jsBoxCurrentStyle = jsBox.getAttribute('style')
        jsBox.setAttribute('style', jsBoxCurrentStyle + "margin-top: 19px")

    ```
