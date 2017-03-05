#INPUT=../ExampleReviews/kiwix/reviews_org.kiwix.kiwixmobile_201606.csv
INPUT=all.csv
OUTPUT=result.pb result.fbs result_fbs.txt result_pb.json

all: $(OUTPUT)

all.csv: head.csv all1.csv #all32.csv
	cat $^ > $@

head.csv: ../ExampleReviews/kiwix/reviews_org.kiwix.kiwixmobile_201606.csv
	head -1 $^ > $@

all1.csv: $(wildcard ../ExampleReviews/kiwix/*.csv)
	cat $^ | iconv -f utf-16 -t utf-8 | grep -v "Package Name" > $@

all2.csv: all1.csv
	cat $^ $^ > $@

all4.csv: all2.csv
	cat $^ $^ > $@

all8.csv: all4.csv
	cat $^ $^ > $@

all16.csv: all8.csv
	cat $^ $^ > $@

all32.csv: all16.csv
	cat $^ $^ > $@

result_fbs.txt: transform.py result.fbs
	/usr/bin/time python $^ $@

result_pb.json: transform.py result.pb
	/usr/bin/time python $^ $@

result.pb: transform.py $(INPUT)
	/usr/bin/time python $^ result.pb 

result.fbs: transform.py $(INPUT)
	/usr/bin/time python $^ result.fbs

transform.py: review_pb2.py review/Data.py
	touch $@

review.fbs: review.proto
	flatc --proto review.proto

review/Data.py review/Review.py: review.fbs
	flatc -p -o . review.fbs

review_pb2.py: review.proto
	protoc -I=. --python_out=. review.proto

clean::
	git ls-files -o | xargs rm -rf
	rm -rf _review
