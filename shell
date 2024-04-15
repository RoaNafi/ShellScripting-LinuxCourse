#roa nafi || alaa shaheen
is_saved=1
cat /dev/null > precoded_feature.txt
exit=1
flag=0 
while [ $exit -eq 1 ]
do
 
	echo "--------------- M E N U ---------------"
	echo "r) read a dataset from file"
	echo "p) print the name of the features"
	echo "l) encode a feature using lebel incoding"
	echo "o) encode a feature using one_hot incoding"
	echo "m) apply MinMax scalling"
	echo "s) save the processed dataset"
	echo "e) exit"
	echo "----------------------------------------"
	read option

	if [ "$option" == "r" ]
	then 
		echo "Enter file name: "
		read original_file
	if [ ! -e "$original_file" ]; then # Checking if file isaccessible 
		echo "File doesn't exist"
	else
#--------------
		flag=1 # make user read file first		
		num_of_features=$(sed -n 1p $original_file | tr ";" " " | wc -w)
		num_of_features_in_firstLine=$(sed -n 2p $original_file | tr ";" " " | wc -w)
		if [[ "$num_of_features" == "$num_of_features_in_firstLine" ]]
		then
		cp $original_file copy_dataset.txt
                file=copy_dataset.txt		 
		else
		echo "the format of the file is wrong"
		fi
	fi
#-----------------------
		#cp $original_file copy_dataset.txt
		#file=copy_dataset.txt
	elif [ "$option" == "p" ] && [ $flag == 1 ] 
	then 
		cat /dev/null > features.txt
		j=1
		num_of_features=$(sed -n 1p $file | tr ";" " " | wc -w)
		while [ $j -le $num_of_features ]
		do
		sed -n 1p $file | cut -d';' -f$j >> features.txt
		j=$(($j+1))
		done
		cat features.txt
	elif [ "$option" == "l" ] && [ $flag == 1 ]
	then
		is_saved=0
		cat /dev/null > lebel_encoded.txt
                cat /dev/null > features.txt
                cat /dev/null > to_encode.txt
                cat /dev/null > uniq_data.txt
                cat /dev/null > not_uniq_data.txt
                cat /dev/null > number.txt 
		j=1
                num_of_features=$(sed -n 1p $file | tr ";" " " | wc -w)
                while [ $j -le $num_of_features ]
                do
                sed -n 1p $file | cut -d';' -f$j >> features.txt
                j=$(($j+1))
                done
		echo "please input the name of the categorical feature"
		read  feature_to_encode
		is_in_file=$(sed -n 1p $file | grep -c $feature_to_encode)
# ensure that feature exists and has not been encoded
if [ $is_in_file -eq 1 ]
	then
	p=1
	flag2=1
	tempp=$(sed -n "$p"p precoded_feature.txt)
	until [ -z $tempp ]
	do    
	if [[ "$tempp" == "$feature_to_encode" ]]
	then
	flag2=0
	break
	else
	p=$(($p+1))
	tempp=$(sed -n "$p"p precoded_feature.txt)
	fi
	done
if [ $flag2 -eq 1 ]
	then
	echo $feature_to_encode >> precoded_feature.txt		
#***************give a number for each value*****************
#e.g yes  1 \n  no  0
	j=$(grep -n $feature_to_encode features.txt | cut -d':' -f1) #num of feature
	# get all data 	
	i=0  #-------- > num of row in dataset 
	k=2
	line=$(sed -n "$k"p $file) #----------> get line by line
until [ -z "$line" ]; #--------> until the end of the file
	do
	echo $line | cut -d';' -f$j >> not_uniq_data.txt
	i=$(($i+1))
	k=$(($k+1))
	line=$(sed -n "$k"p $file)
done
	sort not_uniq_data.txt | uniq > uniq_data.txt 
	r=0
	i2=$(cat uniq_data.txt | wc -l)  #------> get number of lines
while [ $i2 -gt 0 ]
	do
	echo $r >> number.txt
	r=$(($r+1))
	i2=$(($i2-1))
done
	paste -d ' ' uniq_data.txt number.txt > uniq_num_data.txt
#***********************************************************************
#************get all value under a specific feature*********************
	echo $feature_to_encode >> lebel_encoded.txt
	k3=1		
	d=$(grep -n $feature_to_encode features.txt | cut -d':' -f1) # features field 
	linee=$(sed -n "$k3"p $file) 
until [ -z "$linee" ];
	do
	to_encode=$(echo "$linee" | cut -d';' -f$d)
	echo $to_encode >> to_encode.txt
	k3=$(($k3+1))
	linee=$(sed -n "$k3"p $file)
done
#*****************************
#****************************start lable encoded***************************
	k1=2
	line_to_encode=$(sed -n "$k1"p to_encode.txt) #----------> get line by line
until [ -z "$line_to_encode" ]; #--------> until the end of the file
	do
	k2=1
	line_data_num=$(sed -n "$k2"p uniq_num_data.txt) #-> get line by line
                
until [ -z "$line_data_num" ];
	do
	data_encode=$(echo "$line_data_num" | cut -d' ' -f1)	
	if [[ "$line_to_encode" == "$data_encode" ]]
	then
	data_num=$(echo "$line_data_num" | cut -d' ' -f2)
	modfitxt=$(sed "s/$line_to_encode/$data_num/" <<< "$line_to_encode")		
	echo "$modfitxt" >> lebel_encoded.txt
	k2=$(($k2+1))
	else
	k2=$(($k2+1))
	fi
	line_data_num=$(sed -n "$k2"p uniq_num_data.txt)
done 
	k1=$(($k1+1))
	line_to_encode=$(sed -n "$k1"p to_encode.txt)
done
	cut --complement -d';' -f$d $file > temp.txt
	paste temp.txt lebel_encoded.txt > temp2.txt
	sed 's/[[:space:]]/;/g' temp2.txt | sed 's/;;/;/g' | sed 's/$/;/' > $file
	cat uniq_num_data.txt
        rm temp.txt
        rm temp2.txt
        rm lebel_encoded.txt
        rm to_encode.txt
        rm uniq_num_data.txt
        rm not_uniq_data.txt
        rm uniq_data.txt
        rm number.txt
	rm features.txt	
	else 
	echo "it has been encoded"
fi
		else
		echo "the name of categorical feature is wrong "
fi
#*************end lable encoded******************************


	elif [ "$option" == "o" ] && [ $flag == 1 ]
	then
	is_saved=0
        cat /dev/null > features.txt
        cat /dev/null > uniq_data.txt
        cat /dev/null > not_uniq_data.txt
	#get the feaatures from the file
	j=1
        num_of_features=$(sed -n 1p $file | tr ";" " " | wc -w)
while [ $j -le $num_of_features ]
        do
        sed -n 1p $file | cut -d';' -f$j >> features.txt
        j=$(($j+1))
done
	echo "please input the name of the categorical feature"
	read  feature_to_encode
	is_in_file=$(sed -n 1p $file | grep -c $feature_to_encode)
# ensure that feature exists and has not been encoded
if [ $is_in_file -eq 1 ]
	then
	p=1
	flag2=1
	tempp=$(sed -n "$p"p precoded_feature.txt)
until [ -z $tempp ]
	do    
	if [[ "$tempp" == "$feature_to_encode" ]]
	then
	flag2=0
	break
	else
	p=$(($p+1))
	tempp=$(sed -n "$p"p precoded_feature.txt)
	fi
done
if [ $flag2 -eq 1 ] #in if feature has not been encoded
	then
	echo $feature_to_encode >> precoded_feature.txt		
#get data under specific feature
	j=$(grep -n $feature_to_encode features.txt | cut -d':' -f1) #num of feature 	
	i=0  #-------- > num of row in dataset 
	k=2
	line=$(sed -n "$k"p $file) #----------> get line by line
	until [ -z "$line" ]; #--------> until the end of the file
	do
	echo $line | cut -d';' -f$j >> not_uniq_data.txt
	i=$(($i+1))
	k=$(($k+1))
	line=$(sed -n "$k"p $file)
	done
	#get uniq data
	sort not_uniq_data.txt | uniq > uniq_data.txt 
#*****************start hot encoding****************
#get new feature ---> e.g smoke-yes | smoke-no in temp4
cat /dev/null > temp4
l=1
lineee=$(sed -n "$l"p uniq_data.txt)
until [ -z "$lineee" ];
do
echo "$feature_to_encode-$lineee" >> temp4
l=$(($l+1))
lineee=$(sed -n "$l"p uniq_data.txt)
done
#------------------------
echo "" > hot_column.txt
cat /dev/null > temp5
l=1
uniq_line=$(sed -n "$l"p temp4) #e.g smoke-no | smoke-yes
until [ -z "$uniq_line" ];
do
	echo "$uniq_line;" > temp5
	f2_uniq_line=$(echo "$uniq_line" | cut -d'-' -f2) #no
	l2=1
	not_uniq_line=$(sed -n "$l2"p not_uniq_data.txt) #no
	until [ -z "$not_uniq_line" ];
		do
		if [[ "$not_uniq_line" == "$f2_uniq_line" ]] #no=no
		   then
		   echo "1;" >> temp5
		   l2=$(($l2+1))
		   not_uniq_line=$(sed -n "$l2"p not_uniq_data.txt)
		else
		   echo "0;" >> temp5
		   l2=$(($l2+1))
		   not_uniq_line=$(sed -n "$l2"p not_uniq_data.txt)
		fi
	done

	paste  hot_column.txt temp5 > temp6
	sed 's/[[:space:]]//g' temp6 > hot_column.txt  
	l=$(($l+1))
	uniq_line=$(sed -n "$l"p temp4) #smoke-yes
	done
	cut --complement -d';' -f$j $file > temp.txt
	paste temp.txt hot_column.txt | sed 's/[[:space:]]//g' > $file
	cat hot_column.txt
	rm temp.txt
	rm temp4
	rm temp5
	rm temp6
        rm not_uniq_data.txt
        rm uniq_data.txt
        rm features.txt
	rm hot_column.txt 
	else 
	echo "it has been encoded"
	fi
		else
		echo "the name of categorical feature is wrong "
fi
#******************** end hot encoded **************************************
	elif [ "$option" == "m" ] && [ $flag == 1 ]
	then
#get the feaatures from the file
	cat /dev/null > scale.txt
	cat /dev/null > features.txt
	cat /dev/null > not_uniq_data.txt
	j=1
        num_of_features=$(sed -n 1p $file | tr ";" " " | wc -w)
        while [ $j -le $num_of_features ]
        do
        sed -n 1p $file | cut -d';' -f$j >> features.txt
        j=$(($j+1))
        done
	echo "please input the name of the categorical feature"
	read  feature_to_scale
#if f exsits 
	is_in_file=$(sed -n 1p $file | grep -c $feature_to_scale)
	if [ $is_in_file -eq 1 ] # enter if is in file 
	then
#***************get data under specific feature*****************
	j=$(grep -n $feature_to_scale features.txt | cut -d':' -f1) #num of feature 	
	i=0  #-------- > num of row in dataset 
	k=2
	line=$(sed -n "$k"p $file) #----------> get line by line
	until [ -z "$line" ]; #--------> until the end of the file
	do
	echo $line | cut -d';' -f$j >> not_uniq_data.txt
	i=$(($i+1))
	k=$(($k+1))
	line=$(sed -n "$k"p $file)
	done
#***********************************************************************
	from_not_uniq=$(sed -n 1p not_uniq_data.txt)
	re='^[0-9]+$'
	if  [[ $from_not_uniq =~ $re ]] 
	then
#code
	min_value=$(cut -f1 -d" " not_uniq_data.txt | sort -n | head -1)
	max_value=$(cut -f1 -d" " not_uniq_data.txt | sort -n | tail -1)
	echo " "
	echo "=========="
	echo min value = 
	echo $min_value
	
	echo max value =
	echo $max_value
	echo "=========="
	echo "$feature_to_scale scaled" > scale.txt
	k=1
	value=$(sed -n "$k"p not_uniq_data.txt)
	until [ -z "$value" ]; #--------> until the end of the file
		do
		Numerator=$((value - min_value)) 
		denominator=$((max_value - min_value))
		#echo #$((Numerator / denominator))
		echo "scale=4; $Numerator / $denominator ;" | bc >> scale.txt
		k=$(($k+1))
		value=$(sed -n "$k"p not_uniq_data.txt)
		done
	echo $feature_to_scale > temp
	cat not_uniq_data.txt >> temp
	paste -d'	' temp scale.txt > temp2
	cat temp2
	rm temp
	rm temp2
	rm scale.txt
	rm features.txt
	rm not_uniq_data.txt
#----------------------
	else
		echo "this feature is categorical feature and must be encoded first"
	fi
else
	echo "the name of categorical feature is wrong"
fi


#end scale
	elif [ "$option" == "s" ] && [ $flag == 1 ] 
        then
	if [ -s $file ]
	then 
        echo " Please input the name of the file to save the processed dataset"
        read save_in
	cat $file > $save_in
	is_saved=1
	else
	echo "nothing new to saved....!"
	fi
	elif [ "$option" == "e" ]
	then		
		if [ $is_saved -eq 1 ]
		then
			echo "sure you want to exit ? ( y for yes \ any key for no)"
			read e
			if [ "$e" == "y" ]
			then
			rm $file
			rm precoded_feature.txt
			break
			fi

		
		elif [ $is_saved -eq 0 ]
		then
			echo "processed datasete is not saved, sure you want to exit ?( y for yes \ any key for no)"
			read e2
                        if [ "$e2" == "y" ]
                        then
			rm $file
                        exit=0
                        rm precoded_feature.txt
                        fi
		else 
			rm precoded_feature.txt
			break
		fi
	elif [ "$option" == "p" -o "$option" == "l" -o  "$option" == "o" -o "$option" == "r"\) ] && [ $flag == 0 ]
	then
		echo "Please Read the input file first "
	else
		#echo "try to Read the input file first"
		echo "error:  try valid option "
	fi
done
