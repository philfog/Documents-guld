# Guld Economic Update

__Q2 2018__

### Overview

In Q1 the GULD supply increased ~6.3% from 1,010,445 to 1,074,274. The majority of this increase was in grants to the new [Raadyx Legal Organization](https://raadyx.com) and [Beta Town](https://github.com/betatown/Documents) (a Guld-governed experimental town inside [Panama Pacifico](http://panamapacifico.com)). Prices also increased 87.5% from $40/GULD to $75/GULD. The combined effect of the supply and price increases is a market cap increase from $40,417,800 to $80,570,550 or ~100%.

### 90% Registration Fee Reduction

Registration fees were getting expensive in US$ terms. To remedy this, a 90% name registration fee was implemented on April 1, 2018.

__Previous Price Table__

|Category|Keys|Registration Cost|$ Cost|
|------|------|------|------|
|device|user or group as parent|0.1 guld|$7.5|
|user|one parent key|1 guld|$75|
|group|1-10 user(s) as parents|1 guld / member|$75/member|
|medium group|11-100 users as parents|100 guld|$7,500|
|large group|101-1,000 users as parents|1,000 guld|$75,000|
|enterprise group|1,001+ users as parents|10,000 guld|$750,000|

Name registration fees have been reduced by 90%, and previously registered indviduals and groups have been issued matching rebates. Supply impact was re-issuing 18874.8 GULD (~1.8%) mostly to the `spartan` and `betatown` groups.

__New Price Table__

|Category|Keys|Registration Cost|$ Cost|
|------|------|------|------|
|device|user or group as parent|0.01 guld|$0.75|
|user|one parent key|0.1 guld|$7.5|
|group|1-10 user(s) as parents|0.1 guld / member|$7.5/member|
|medium group|11-100 users as parents|10 guld|$750|
|large group|101-1,000 users as parents|100 guld|$7,500|
|enterprise group|1,001+ users as parents|1,000 guld|$75,000|

##### individual fee recovery transaction

```
2018/04/01 * individual registration fee rebate
    Username:Assets   0.9 x GULD
    Username:Income:guld:register:fee-rebate   -0.9 x GULD
    guld:Liabilities   -0.9 GULD
    guld:Income:register:individual:Username   0.9 GULD
```

##### group fee recovery transaction

```
2018/04/01 * group registration fee rebate
    groupname:Assets   m * 0.9 GULD
    groupname:Income:guld:register:fee-rebate   -m * 0.9 GULD
    guld:Liabilities   -m * 0.9 GULD
    guld:Income:register:group:groupname   m * 0.9 GULD
```

These transactions were run a single time on April 1, 2018 and already rebated the individual or group that registered each name.
