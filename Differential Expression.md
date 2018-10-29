Move to Home directory```cd $HOME```

Load the R module to the environment ```module load R/3.5.0```

Open R``` R```

Write code in R

```
library(edgeR)
exp <- read.delim("ERR1146950_ERR1146955_total_counts.txt", header=TRUE, row.names="GeneID")

rcgrp <- factor(c(3,1,1,1,2,2,2))
y <- DGEList(counts = exp, group = rcgrp)
y$samples
design <- model.matrix(~rcgrp)
y <- estimateGLMCommonDisp(y,design)
y <- estimateGLMTrendedDisp(y,design)
y <- estimateGLMTagwiseDisp(y,design)

TestDE <- exactTest(y, pair = c(2,1))``` binomial distribusion 
topTags(TestDE)
deTestDE <- decideTestsDGE(TestDE, adjust.method = "BH", p.value = 0.05)
summary(deTestDE)
table_TestDE <- topTags(TestDE, n=55986) ''' n=genes of interested species
write.csv(table_TestDE, "DE_ERR1146950_ERR1146955.csv")
quit()
```
