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

## 1.format.html

```ruby
# app/views/posts/index.html.erb
<%= link_to 'Show(html)', post, id: "post_ajax_html" %>

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

## 2.format.js
```
# app/views/posts/index.html.erb
<%= link_to 'Show(js-remote)', post, remote: true %>

# app/views/posts/show.js.erb
$("#post_area").html("<%= j(render partial: 'post', locals: { post: @post }) %>")

# 需要建立一個 _post.html.erb 檔，內容可以與show.html.erb 相同
```

## 3.format.json
```
# app/views/posts/index.html.erb
<%= link_to 'Show(json)', post, id: "post_ajax_json" %>

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



