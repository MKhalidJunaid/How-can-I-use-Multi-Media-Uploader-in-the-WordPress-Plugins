How-can-I-use-Multi-Media-Uploader-in-the-WordPress-Plugins
===========================================================

How can I use Multi Media Uploader in the WordPress Plugins

Here is the example how you can use the wordpress multimedia uploader for more than one field or as many fields. The JS code and html code looks like this

How it works

Add upload_image_button class on each upload button on the basis of this class click function triggers to fetch the wordpress multimedia uploader , see i have used the prev() property to get the previous element to the clicked one formfieldID=jQuery(this).prev().attr("id"); and then i have assigned the image url returned by uploader to formfieldID

	 <input id="upload_image1" type="text" name="upload_image1" value="<?php echo $upload_image1;?>" />
	    <input class="upload_image_button" type="button" value="Upload image 1" />
	
	<input id="upload_image2" type="text"  name="upload_image2" value="<?php echo $upload_image2;?>" />
	    <input class="upload_image_button" type="button" value="Upload image 2" />
	
	<script type="text/javascript">
	    //tb_show('', 'media-upload.php?TB_iframe=true');
	    var upload_image_button=false;
	    jQuery(document).ready(function() {
	
	    jQuery('.upload_image_button').click(function() {
	        upload_image_button =true;
	        formfieldID=jQuery(this).prev().attr("id");
	     formfield = jQuery("#"+formfieldID).attr('name');
	     tb_show('', 'media-upload.php?type=image&amp;TB_iframe=true');
	        if(upload_image_button==true){
	
	                var oldFunc = window.send_to_editor;
	                window.send_to_editor = function(html) {
	
	                imgurl = jQuery('img', html).attr('src');
	                jQuery("#"+formfieldID).val(imgurl);
	                 tb_remove();
	                window.send_to_editor = oldFunc;
	                }
	        }
	        upload_image_button=false;
	    });
	
	
	    })
	
	</script>
Also make sure you have included the necessary JS and CSS files of the uploader for this you have to add an action

	 <?php
	 function my_admin_uploader_scripts() {
	 wp_enqueue_script('media-upload');
	 wp_enqueue_script('thickbox');
	 wp_register_script('my-upload', WP_PLUGIN_URL.'/my-script.js', array('jquery','media-upload','thickbox'));
	 wp_enqueue_script('my-upload');
	 }
	
	 function my_admin_uploader_styles() {
	 wp_enqueue_style('thickbox');
	 }
	 add_action('admin_print_scripts', 'my_admin_uploader_scripts');
	 add_action('admin_print_styles', 'my_admin_uploader_styles'); 
	 ?>
Note: Also take care of one thing when you assign the uploader to you input fields the control of uploader now assigns to the fields so when ever uploader will be called it assigns the image url to your text field , so this is the problem when you triggers the uploader you are unable to insert the images in the content area of the post for this solution you have to store the previous control in some where and when you are done with uploader then reassign the control of uploader to the previous function which i have provided in my JS code see below how it works
	
	  var oldFunc = window.send_to_editor;
	  window.send_to_editor = function(html) {
	
	  imgurl = jQuery('img', html).attr('src');
	  jQuery("#"+formfieldID).val(imgurl);
	   tb_remove();
	  window.send_to_editor = oldFunc;
	  }
I have used this technique many times and it works perfect for me

Hope it makes sense
