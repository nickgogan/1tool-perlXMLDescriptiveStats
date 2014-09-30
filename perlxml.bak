#!/usr/bin/perl 
use strict; 
use warnings; 
use diagnostics; 

my $counter = 1;
my $source = "O:\\xml-examples"; #Directory of XML files
my $dest = "$source" . "\\results"; #Directory of future .txt results files

#Scan given directory (currently hardcoded, but can use @ARGV[0]) for .xml files and no other files or directories.
opendir(XMLFILES, $source) || die "Can't open $source: \n$!\n";
my @xmls; #List of the XML docs in the given directory(ies)
my @txts; #List of the .txt corresponding to each XML doc

mkdir($dest) unless (-d $dest);

while(my $filename = readdir(XMLFILES) ) {
	#Make a results directory within the given XML directory, unless it already
	#contains one.
	if ($filename =~ m#[.]xml$#i) {
		(my $basename = $filename) =~ s#[.]xml$##i;
		$basename .= "_results.txt"; #remove .xml and append _result.txt instead
		#print "$counter\t$basename\n";
		open(FILE, ">$dest/$basename"); #create empty .txt files in the results directory for each XML file
		push(@xmls, $source."\\".$filename); #Create an array of the xml file paths
		push(@txts, $dest."\\".$basename);
	}
	
}
closedir(XMLFILES); #Don't forget to close the opened directory(ies)!

#print "\n@txts\n";

#Time to work on each individual xml file and append the results to the empty _results.txt files 
for(my $i = 0; $i < ($#xmls+1); $i++) {
	#my $xml = $xmls[0]; #$i
	
	#Open the given file, now known as FILE
	open(FILE, "<", $xmls[$i]) || die "Can't open file $xmls[$i]: \n$!\n";
	open(RESULT, ">", $txts[$i]) || die "Can't open file $txts[$i]: \n$!\n";
	while(my $line = <FILE>) { #While reading the given FILE line-by-line:
		$counter++; #XML file F1 gives 3 lines!!
		while ($line =~ m#<[^>]+>#sg) { #While the given line in the given file matches: <anything but >>
			my $tag = $&; #Create a scalar variable to hold the string of the line that matches the regex
			if ($tag  !~ m#</#) { #If that particular match is NOT as close tag 
				if ($tag =~ m#<(\w+)#si) { #And if that particular match also matches: <some_word
					my $tagname = $1; #Extract the name of that tag, which is the first word in the tag
					print RESULT "<$tagname\n";
					my $rest = $'; #This scalar holds the remaining parameters and their values (if any) of the tag
					#print "my tag: $tagname\n"; 
					my %results = ext($rest); #Create a hash to hold the attribute-value pairs of the given tagname
					#This part will separate the attributes and values strings and store them in different variables
					foreach my $key (keys(%results)) { #For each key in %results (sorted first),  
						my $val = $results{$key};
						print RESULT "$key\t=\t\"$val\"\n";
					}
					print RESULT ">\n\n";
				}

			}

		}
	}
	close(FILE);
	close(RESULT);
}

#Extracts the attributes-values of an XML tag
sub ext {
	my ($attr) = @_;
	my %hash;
	while($attr =~ m#\s+([^=]+)="([^"]+)"#s) {
		$attr = $';
		my $att = $1;
		my $val = $2;
		$hash{$att} =  $val;
	}
	return %hash;
}

exit();



