berkas
======
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Untitled Form</title>
<link rel="stylesheet" type="text/css" href="./js/view.css" media="all">
<script type="text/javascript" src="./js/view.js"></script>

</head>
<body id="main_body" >
	
<?
include"../koneksi.php";

$id=$_GET['id'];
$edit="select * from penduduk where id='$id'";
$cek=mysql_query($edit) or die (mysql_error()); 
$data=mysql_fetch_array($cek) ;?>	
	<div id="form_container">
	
		<h1><a>Pendaftaran Penduduk</a></h1>
		<form id="form_710451" class="appnitro" enctype="multipart/form-data" method="post" action="">
					<div class="form_description">
			<h2>Form Pendaftaran Penduduk</h2>
			<p></p>
		</div>						
			<ul >
			
		
		<li id="li_2" >

		<div class="left">
			<label class="description" for="element_4">Nama </label>
			<input id="element_4_1" name="nama" class="element text large" value="<?echo $data['nama']?>" type="text">			
		</div>				
		<div class="right">
			<label class="description" for="element_4">Alamat </label>
			<input id="element_4_1" name="alamat" class="element text large" value="<?echo $data['alamat']?>" type="text">	
					
		</div>		
		<div class="left">
			<label class="description" for="element_2">Desa </label>
			<input id="element_2" name="desa" class="element text medium" type="text" maxlength="255" value="<?echo $data['desa']?>"/> 
		</div>
		<div class="right">
			<label class="description" for="element_2">Kecamatan </label>
			<input id="element_2" name="kecamatan" class="element text medium" type="text" maxlength="255" value="<?echo $data['kecamatan']?>"/> 
		</div> 
		<div class="right">
			<label class="description" for="element_2">No Kartu Tanda Penduduk </label>
			<input id="element_2" name="noktp" class="element text medium" type="text" maxlength="255" value="<?echo $data['noktp']?>"/> 
		</div>  
		<div class="left">
			<label class="description" for="element_2">No Kartu Keluarga </label>
			<input id="element_2" name="nokk" class="element text medium" type="text" maxlength="255" value="<?echo $data['nokk']?>"/> 
		</div>  
		
		<li id="li_5" >
		<label class="description" for="element_5">Upload Berkas Penduduk </label>
		<div>
			<input id="element_5" name="userfile" class="element file" type="file" value="<?echo $data['ketrt']?>"/> 
		</div>  
		<div>
			<label class="description" for="element_2">Keterangan Kelangkapan Berkas </label>
			<textarea id="element_5" name="ket" class="element file" type="text" value="<?echo $data['ket']?>"/></textarea> 
		</div>  
		</li>
			
					<li class="buttons">
			    
			    
				<?if($data){echo"<input type='submit' name='update' value='Update'><a href='admin.php'>View</a>";}else{echo"<input type='submit' name='save' value='Save'>"; 
				header('location:admin.php');}?>
		</li>
			</ul>
		</form>	
		
	</div>
	<img id="bottom" src="bottom.png" alt="">
	</body>
</html>

<?//include"tampil.php";?>	
<!--end form-->
<?php
//=============================================update
$id=$_GET['id'];
if (isset($_POST['update']))
{
   
		$uploaddir = 'data/';
		$fileName = $_FILES['userfile']['name'];  
		$tmpName  = $_FILES['userfile']['tmp_name']; 
		$fileSize = $_FILES['userfile']['size'];
		$fileType = $_FILES['userfile']['type'];
			
		$query = "SELECT count(*) as jum FROM upload WHERE name = '$fileName'";
		$hasil = mysql_query($query);
		$data  = mysql_fetch_array($hasil);
		$query = "UPDATE penduduk SET nama='$_POST[nama]', alamat='$_POST[alamat]', desa='$_POST[desa]', kecamatan='$_POST[kecamatan]', noktp='$_POST[noktp]', nokk='$_POST[nokk]', ketrt = '$fileName',  
                ket='$_POST[ket]' where id='$id'";
		   $query1 = mysql_query($query);
		$uploadfile = $uploaddir . $fileName;
				if (move_uploaded_file($_FILES['userfile']['tmp_name'], $uploadfile)) 
				{ echo "<script>alert('Thank data Successfull updated!!');</script>"; 
 
				} else {   echo "<script>alert('Update Failed, Tray again !!');</script>";}
}



//==============================================save
if(isset($_POST['save'])){
		// setting nama folder tempat upload
		$uploaddir = 'data/';

		// membaca nama file yang diupload
		$fileName = $_FILES['userfile']['name'];  
		$tmpName  = $_FILES['userfile']['tmp_name']; 
		$fileSize = $_FILES['userfile']['size'];
		$fileType = $_FILES['userfile']['type'];

			
		include"./koneksi.php";

		// menyimpan properti atau informasi file ke tabel upload dalam db
		// dengan terlebih dahulu mengecek ada tidaknya nama file dalam tabel



		$query = "SELECT count(*) as jum FROM upload WHERE name = '$fileName'";
		$hasil = mysql_query($query);
		$data  = mysql_fetch_array($hasil);

		$query = "INSERT INTO penduduk (nama, alamat, desa, kecamatan, noktp, nokk, ketrt, ket) VALUES ('$_POST[nama]','$_POST[alamat]','$_POST[desa]','$_POST[kecamatan]','$_POST[noktp]',
                '$_POST[nokk]','$fileName', '$_POST[ket]')";
		mysql_query($query);

		// menggabungkan nama folder dan nama file
		$uploadfile = $uploaddir . $fileName;

		// proses upload file ke folder 'data'
				if (move_uploaded_file($_FILES['userfile']['tmp_name'], $uploadfile)) 
				{echo "<script>alert('Terima Kasih sudah melakukan entry data, Successfull !!');</script>";} 
				else {echo "<script>alert('Entry Failed, Tray again !!');</script>".(mysql_error());}
			}

?>
