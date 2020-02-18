# shopping-cart-dynamic-number
When building out shopping carts, you likely want to give users the ability to change the quantity of a selected item. With using the number type input and some javascript, you can dynamically update a cart total depending upon user selection.

## Tutorial
For detailed instruction's, view Solodev's [How to use “number” type inputs to dynamically change your shopping cart total](https://www.solodev.com/blog/web-design/how-to-use-number-type-inputs-to-dynamically-change-your-shopping-cart-total.stml

## Demo
Try out a working example on [JSFiddle](https://jsfiddle.net/solodev/o7gfaps4/).

## HTML
The tutorial contains the following basic HTML markup.

```
<div class="container mt-5 mb-5">
      <div class="row justify-content-center">
        <div class="col-xl-7 col-lg-8 col-md-7">
          <div class="border border-gainsboro p-3">

            <h2 class="h6 text-uppercase mb-0">Cart Total (2 Items): <strong class="cart-total"></strong></h2>
          </div>

          <div class="border border-gainsboro p-3 mt-3 clearfix item">
            <div class="text-lg-left">
              <i class="fa fa-ticket fa-2x text-center" aria-hidden="true"></i>
            </div>
            <div class="col-lg-5 col-5 text-lg-left">
              <h3 class="h6 mb-0">SpaceJet Flex ticket<br>
                <small>Cost: $50.00/ea</small>
              </h3>
            </div>
            <div class="product-price d-none">50</div>
            <div class="pass-quantity col-lg-3 col-md-4 col-sm-3">
              <label for="pass-quantity" class="pass-quantity">Quantity</label>
              <input class="form-control" type="number" value="1" min="1">
            </div>
            <div class="col-lg-2 col-md-1 col-sm-2 product-line-price pt-4">
              <span class="product-line-price">50
              </span>
            </div>
            <div class="remove-item pt-4">
              <button class="remove-product btn-light">
                Delete
              </button>
            </div>
          </div>

          <div class="border border-gainsboro p-3 mt-3 clearfix item">
            <div class="text-lg-left">
              <i class="fa fa-ticket fa-2x text-center" aria-hidden="true"></i>
            </div>
            <div class="col-lg-5 col-5 text-lg-left">
              <h3 class="h6 mb-0">SpaceJet Flex ticket 2<br><small><small>Cost: $45.00/ea</small></small></h3>
            </div>
            <div class="product-price d-none">45.00</div>
            <div class="pass-quantity col-lg-3 col-md-4 col-sm-3">
              <label for="pass-quantity" class="pass-quantity">Quantity</label>
              <input class="form-control" type="number" value="1" min="1">
            </div>
            <div class="col-lg-2 col-md-1 col-sm-2 product-line-price pt-4">
              <span class="product-line-price">45.00</span>
            </div>
            <div class="remove-item pt-4">
              <button class="remove-product btn-light">
                Delete
              </button>
            </div>
          </div><!-- item -->
        </div>

        <div class="col-xl-3 col-lg-4 col-md-5 totals">
          <div class="border border-gainsboro px-3">
            <div class="border-bottom border-gainsboro">
              <p class="text-uppercase mb-0 py-3"><strong>Order Summary</strong></p>
            </div>
            <div class="totals-item d-flex align-items-center justify-content-between mt-3">
              <p class="text-uppercase">Subtotal</p>
              <p class="totals-value" id="cart-subtotal"></p>
            </div>
            <div class="totals-item d-flex align-items-center justify-content-between">
              <p class="text-uppercase">Estimated Tax</p>
              <p class="totals-value" id="cart-tax"></p>
            </div>
            <div class="totals-item totals-item-total d-flex align-items-center justify-content-between mt-3 pt-3 border-top border-gainsboro">
              <p class="text-uppercase"><strong>Total</strong></p>
              <p class="totals-value font-weight-bold cart-total"></p>
            </div>
          </div>
          <a href="#" class="mt-3 btn btn-pay w-100 d-flex justify-content-between btn-lg rounded-0">Pay Now <span class="totals-value cart-total"></span></a>
        </div>

      </div>
    </div><!-- container -->
```

## CSS
All required CSS is contained with style.css

## Javascript
```
 $(document).ready(function() {

   /* Set rates */
   var taxRate = 0.05;
   var fadeTime = 300;

   /* Assign actions */
   $('.pass-quantity input').change(function() {
     updateQuantity(this);
   });

   $('.remove-item button').click(function() {
     removeItem(this);
   });


   /* Recalculate cart */
   function recalculateCart() {
     var subtotal = 0;

     /* Sum up row totals */
     $('.item').each(function() {
       subtotal += parseFloat($(this).children('.product-line-price').text());
     });

     /* Calculate totals */
     var tax = subtotal * taxRate;
     var total = subtotal + tax;

     /* Update totals display */
     $('.totals-value').fadeOut(fadeTime, function() {
       $('#cart-subtotal').html(subtotal.toFixed(2));
       $('#cart-tax').html(tax.toFixed(2));
       $('.cart-total').html(total.toFixed(2));
       if (total == 0) {
         $('.checkout').fadeOut(fadeTime);
       } else {
         $('.checkout').fadeIn(fadeTime);
       }
       $('.totals-value').fadeIn(fadeTime);
     });
   }


   /* Update quantity */
   function updateQuantity(quantityInput) {
     /* Calculate line price */
     var productRow = $(quantityInput).parent().parent();
     var price = parseFloat(productRow.children('.product-price').text());
     var quantity = $(quantityInput).val();
     var linePrice = price * quantity;

     /* Update line price display and recalc cart totals */
     productRow.children('.product-line-price').each(function() {
       $(this).fadeOut(fadeTime, function() {
         $(this).text(linePrice.toFixed(2));
         recalculateCart();
         $(this).fadeIn(fadeTime);
       });
     });
   }

   /* Remove item from cart */
   function removeItem(removeButton) {
     /* Remove row from DOM and recalc cart total */
     var productRow = $(removeButton).parent().parent();
     productRow.slideUp(fadeTime, function() {
       productRow.remove();
       recalculateCart();
     });
   }

 });
```

## External Resources
This tutorial includes the following third party resources.

```
<link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.4.1/css/bootstrap.min.css" integrity="sha384-Vkoo8x4CGsO3+Hhxv8T/Q5PaXtkKtu6ug5TOeNV6gBiFeWPGFN9MuhOf23Q9Ifjh" crossorigin="anonymous">
<link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/font-awesome/4.7.0/css/font-awesome.min.css">
<script src="//cdnjs.cloudflare.com/ajax/libs/jquery/3.2.1/jquery.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/popper.js@1.16.0/dist/umd/popper.min.js" integrity="sha384-Q6E9RHvbIyZFJoft+2mJbHaEWldlvI9IOYy5n3zV9zzTtmI3UksdQRVvoxMfooAo" crossorigin="anonymous"></script>
<script src="https://stackpath.bootstrapcdn.com/bootstrap/4.4.1/js/bootstrap.min.js" integrity="sha384-wfSDF2E50Y2D1uUdj0O3uMBJnjuUD4Ih7YwaYd1iqfktj0Uod8GCExl3Og8ifwB6" crossorigin="anonymous"></script>
```
