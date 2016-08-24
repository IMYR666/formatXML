use Time::HiRes qw ( time alarm sleep ) ;
my $begin = time();

use strict;
use warnings;
use Data::Dumper;
use Encode;
use JSON;
use feature qw(say);

my $dictName = "swg_xhzd";
my $root     = "E:/dict/$dictName/";
my $in       = $root . $dictName . "_test.xml";
my $log      = $root . $dictName . "_log.txt";
my $out     = $root . $dictName . "_origin_format.xml";

open IN,   "<", $in   or die $!;
open OUT, ">",  $out  or die $!;

#open LOG, ">", $log or die $!;

my $indentMark = "\t";
my $indentNum = 1;

my $indentDeep = 0;	
my $content = "";
 
while(<IN>){
	chomp;
	my $line = $_;
	$line =~ s/^ +//g;
	if($line =~ /^[类rik]/){#特殊处理
		$line = ' '.$line;
	}
	$content .= $line;
}

my @contents = ();
if($content =~ /<(.+?)>/){
	$content =~ s/(<.+?>)/<yangrui>$1<yangrui>/g;
	$content =~ s/(<yangrui>)+/<yangrui>/g;
	$content =~ s/^<yangrui>|<yangrui>$//g;
	@contents = split /<yangrui>/, $content;
}
#say Dumper \@contents;
#exit;
my $indent = "";
my $flag = "None";
my @tags = ();
for(my $i = 0; $i < @contents; $i++){
	next if $contents[$i] =~ /<\?.+?\?>/;
	next if $contents[$i] =~ /<!DOCTYPE/;
	if($contents[$i] =~ /<\/(.+?)>/){#结束标签
		#判断标签是否配对	
		isPairOfTag($1);
		
		if($flag eq "eTag" || $flag eq "content"){
			$indentDeep--;
			$indent = getIndent($indentDeep);
		}
		$flag = "eTag";
	}elsif($contents[$i] =~ /<.+?\/>/){
		
	}elsif($contents[$i] =~ /<(.+?)>/){#开始标签
		my $tmp = $1;
		$tmp =~ s/^(.+?) .+$/$1/ if $tmp =~ / /;
		push(@tags,$1);
		
		if($flag eq "bTag"){
			$indentDeep++;
			$indent = getIndent($indentDeep);
		}
		$flag = "bTag";
	}else{#纯文本内容
		if($flag eq "bTag"){
			$indentDeep++;
			$indent = getIndent($indentDeep);
		}
		$flag = "content";
	}
	say OUT $indent.$contents[$i];
}	

sub isPairOfTag{
	my $rTag = shift;
	my $lTag = pop @tags;
	if($rTag ne $lTag){
		say $lTag."<---->".$rTag;
		say "error!!";
		exit;
	}
}
sub getIndent{
	my ($deep) = @_;
	return "" if $deep == 0;
	my $num = $deep * $indentNum;
	my $format = "%0".$num."s";
	my $indent = sprintf($format,$indentMark); 
	$indent =~ s/0/$indentMark/g;
	
	return $indent;
}
my $end = time();
my $run = sprintf("%.2f",$end - $begin);
print "运行时间：".$run."秒\n";	
say $run;
print "finish!!!\n";
