# Eccomerce-tips


<h3>How To Create Add To Cart In Python Django Using Django-Shopping-Cart</h3>
<hr>

<h4>Step 1 :- Install Django Shopping Cart using Pip</h4>
<p>pip install django-shopping-cart</p>
<hr>

<h4>
  Step 2 :- Add "cart" To your INSTALLED APP In settings.py 
</h4>
<p>
  INSTALLED_APPS = [
    
    'cart',
]

Add below line to settings context_processor::
    'cart.context_processor.cart_total_amount'

CART_SESSION_ID = 'cart'
</p>

<hr>

<h4>Step 3 :- You Can use the urls in following Way </h4>
<p>
    path('cart/add/<int:id>/', views.cart_add, name='cart_add'),<br>
    path('cart/item_clear/<int:id>/', views.item_clear, name='item_clear'),<br>
    path('cart/item_increment/<int:id>/',views.item_increment, name='item_increment'),<br>
    path('cart/item_decrement/<int:id>/',views.item_decrement, name='item_decrement'),<br>
    path('cart/cart_clear/', views.cart_clear, name='cart_clear'), <br>
    path('cart/cart-detail/',views.cart_detail,name='cart_detail'),<br>
</p>
<hr>
      
<h4>Step 4 :- This right in views.py file.</h4>
<pre>
  from django.contrib.auth.decorators import login_required
  from cart.cart import Cart

@login_required(login_url="/users/login")
def cart_add(request, id):
    cart = Cart(request)
    product = Product.objects.get(id=id)
    cart.add(product=product)
    return redirect("home")


@login_required(login_url="/users/login")
def item_clear(request, id):
    cart = Cart(request)
    product = Product.objects.get(id=id)
    cart.remove(product)
    return redirect("cart_detail")


@login_required(login_url="/users/login")
def item_increment(request, id):
    cart = Cart(request)
    product = Product.objects.get(id=id)
    cart.add(product=product)
    return redirect("cart_detail")


@login_required(login_url="/users/login")
def item_decrement(request, id):
    cart = Cart(request)
    product = Product.objects.get(id=id)
    cart.decrement(product=product)
    return redirect("cart_detail")


@login_required(login_url="/users/login")
def cart_clear(request):
    cart = Cart(request)
    cart.clear()
    return redirect("cart_detail")


@login_required(login_url="/users/login")
def cart_detail(request):
    return render(request, 'cart/cart_detail.html')
</pre>

<hr>

<h4> Step 5 :- In the templates you can use the url in following way</h4>
<p>
  <a href="{% url 'cart_add' product.id %}">Add To Cart</a>
  <a href="{% url 'cart_clear' %}">Clear Cart</a>
  <a href="{% url 'item_increment' value.product_id %}">Increament</a>
  <a href="{% url 'item_decrement' value.product_id %}">Decrement</a>
  <a href="{% url 'item_clear' key %}">Item clear</a>
</p>

<hr>
<h4>
  Step 6 :- To view the cart detail page use the below code.
</h4>
<pre>
  # Load Cart Tag
{% load cart_tag %}

{% for key,value in request.session.cart.items %}

   {{value.name}} 
   {{value.price}} 
   {{value.quantity}} 
   {{value.image}} 
   Total 
   {{ value.price|multiply:value.quantity }}
 
{% endfor %}
</pre>
