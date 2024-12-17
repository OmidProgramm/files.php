<?php
  if($_SERVER['REQUEST_METHOD'] == 'POST'){

    $maxFiles = 5;
    $maxSize = 2 * 1024*1024;
    $allowedTypes = ['image/jpg', 'image/png', 'image/gif', 'image/jpeg'];
    $totalFiles = count($_FILES['files']['name']);
    if($totalFiles > $maxFiles){
        echo "just 5 files is allowed<br><br>";
    }else{
        for($i=0;$i<$totalFiles;$i++){
            $fileTmp = $_FILES['files']['tmp_name'][$i];
            $fileName = basename($_FILES['files']['name'][$i]);
            $fileSize = $_FILES['files']['size'][$i];
            $fileType = mime_content_type($fileTmp);
            if(!in_array($fileType, $allowedTypes)){
                echo "Files is invalid<br><br>";
            }elseif($fileSize> $maxSize){
                echo "Files is to larg.<br><br>";
            }else{
                $uniqueName = uniqid("img_", true)."_".$fileName;
                $destination = "uploads/".$uniqueName;

                if(move_uploaded_file($fileTmp, $destination)){
                    echo "Files are moved<br><br>";
                }else{
                    echo "upload is errors<br><br>";
                }
            }
        }
    }
       
    

  }else{
    echo "PLS send your FILES<br><br>";
  }
  
  $fileToDelete = $_POST['delete'];
    $filePath = "uploads/".$fileToDelete;
    if(file_exists($filePath))
    {
        if(unlink($filePath))
        {
            echo "File is deleted<br><br>";
        }
    }


?>
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <form action="" method="POST" enctype="multipart/form-data">
        <label for="files">FILES</label>
        <input type="file" name="files[]" id="files" multiple><br><br>
        <input type="submit" value="Send">
    </form>
    <form action="" method="POST">
        <label for="delete">DELETE</label>
        <input type="text" name="delete" id="delete"><br><br>
        <input type="submit" value="DELETE">
    </form>
</body>
</html>
