- 👋 Hi, I’m @Shaharch2016
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...

<!---
Shaharch2016/Shaharch2016 is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->

#!/usr/bin/perl

# use CGI module
use CGI;
use File::Path;

# define location where uploaded files will be stored, notice that permissions to write are needed for upload directory
$uploaddir = '/var/uploads/';
$query = new CGI;

# parse the name of file
$filename = $query->param("file");

# Index where starts the file name   
$index = rindex($filename,"\\");

# Gets the directory  path name from the file name
$dirName = substr($filename, 0,$index+1);

# Check the directory name
if($dirName ne ""){

   # Concat the directory path name        
   $uploaddir =  $uploaddir . $dirName;

   # Make the directory if it does not exist
   if(! -d $uploaddir){
          mkpath $uploaddir;
   }
}

$filename =~ s/.*[\/\\](.*)/$1/;

# gets file handle
$upload_filehandle = $query->upload("file");

# reads contents and saves it out
open UPLOADFILE, ">$uploaddir/$filename" || &upload_error;

# tells Perl to write the file in binary mode, rather than in text mode.
binmode UPLOADFILE;

while ( <$upload_filehandle> ) {
   print UPLOADFILE;
}

# close the file
close UPLOADFILE;

print $query->header();
print "RESP.100";

sub upload_error {
   print INFO "****ES ERROR***\n";        
   print "RESP.200";
   exit;
}
