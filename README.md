== Rails + Ajax

根據respond_to 三種format，介紹三種 ajax 操作方式

```ruby
# app/controllers/posts_controller.rb
def show
  respond_to do |format|
    format.html
    format.js
    format.json { render json: @post }
  end
end
```

<<<<<<< HEAD:README.md
## 1.format.html
=======
### 1. format.html
>>>>>>> e747cefc1220277a64ac2345c2cd2e7306d4e3af:README.rdoc

```ruby
# app/views/posts/index.html.erb
<%= link_to 'Show(html)', post, id: "post_ajax_html" %>
```
```javascript
<script data-turbolinks="false">
  $("#post_ajax_html").click(function(e){
    e.preventDefault();
    let url = $(this).attr("href");
    $.ajax({
      url: url,
      success: (resp) => {
        $("#post_area").html(resp);
      }
    })
  })
</script>
```

### 2. format.js

```ruby
# app/views/posts/index.html.erb
<%= link_to 'Show(js-remote)', post, remote: true %>

# 建立一個 _post.html.erb 檔，內容可以與show.html.erb 相同
# app/views/posts/show.js.erb
$("#post_area").html("<%= j(render partial: 'post', locals: { post: @post }) %>")
```


### 3. format.json

```ruby
# app/views/posts/index.html.erb
<%= link_to 'Show(json)', post, id: "post_ajax_json" %>
```
```javascript
<script data-turbolinks="false">
$("#post_ajax_json").click(function(e) {
  e.preventDefault();
  let url = $(this).attr("href");
  $.ajax({
    url: url,
    dataType: 'json',
    success: (resp) => {
      let content = "<div> Title: "+resp.title+"</div>";
      content += "<div> Description: " + resp.description + "</div>";
      $("#post_area").html(content);
    }
  })
})
</script>
```
