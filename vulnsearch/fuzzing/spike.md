# თეორია

წყარო კოდში სისუსტეების ძებნისას უნდა გავითვალისწინოთ რომელ პროგრამირების ენაშია დაწერილი წყარო კოდი, იმიტომ რომ ბაგები დამოკიდებულია პროგრამირების ენაზე, აგრეთვე ლოგიკურ შეცდომებზე.


## პრაქტიკა

### PHP

#### SPIKE (fuzzer)

**მაგალითი:** test_file.php ფაილში არსებული სისუსტეების ძებნა

```
php /path/to/spike_phpSecAudit/run.php --src test_file.php
```

**მაგალითი:** dir_test პაპკაში მყოფ ფაილებში არსებული სისუსტეების ძებნა

```
php /path/to/spike_phpSecAudit/run.php --src dir_test
```

