Display Featured Products on Page Template
=========================
### Display Featured Products that Have the Attribute of "featured" Set to YES


####Step 1: Create "featured" Attribute
--------------------------------------------
1. Go to Catalog->Attributes->Manage Attributes
2. Add New Attribute with the Attribute Code "featured" and Catalog Input Type of "Yes/No"


####Step 2: Create Template File
--------------------------------------------
```php
<div id="home-featured">
  <?php
		// MAGE HELPERS
		$_helper = $this->helper('catalog/output');
		$storeId = Mage::app()->getStore()->getId();
		$catalog = $this->getLayout()->createBlock('catalog/product_list')->setStoreId($storeId);

		// Retrive Products Marked as Featured
		$collection = Mage::getModel('catalog/product')->getCollection();
		$collection->addAttributeToSelect('featured');
		$collection->addFieldToFilter(array(
			array('attribute' => 'featured', 'eq' => true),
		));
	?>

	<!-- If no products are currently featured, display some text -->
	<?php if (!$collection->count()) :?>	

		<p class="note-msg"><?php echo $this->__('There are currently no featured products.') ?></p>
	<!-- If Products do exsits -->
	<?php else : ?>

	<div class="featured-products">
		<?php foreach ($collection as $_product) :?>
		<?php $_product = Mage::getModel('catalog/product')->setStoreId($storeId)->load($_product->getId()); ?>
		<?php $description = $_product[description]; ?> 
			<div class="featured-product cf">
		        <div class="prod-img"><img src="<?php echo $this->helper('catalog/image')->init($_product, 'small_image')->resize(167,159); ?>" width="167" height="159" alt="<?php echo $this->stripTags($this->getImageLabel($_product, 'small_image'), null, true) ?>" /></div>
		        <div class="prod-info">
		            <h3 class="prod-title"><?php echo $_helper->productAttribute($_product, $_product->getName(), 'name') ?></h3>
		            <p class="prod-description"> <?php echo substr( $description, 0, strpos($description, ' ', 85) ); ?>...</p>
		            <a href="<?php echo $_product->getProductUrl() ?>" class="buy-btn"><span class="green">Buy Now</span></a>                              
		        </div>
		    </div>
		<?php endforeach ?>
	</div>
	<?php endif ?>
</div>
```

####Step 3:Set Block to Desired Location Through XML:
--------------------------------------------

Example:

```xml
<cms_index_index>
  <reference name="content">
      <block type="core/template" name="home-featured"  as="home_featured" template="custom/featured_products.phtml"/>
    </reference>
</cms_index_index>
```

