---
layout: down
title: Release Notes
---

## Upcoming Release 0.4

### Reworked DaleqSupport
    
Up to now, DaleqSupport and its implementation DbUnitDaleqSupport
were not very clear of how they would like to be instantiated. On
one hand, DbUnitDaleqSupport provided some factory methods, which
exposed some of the internal used classes, one the other hand
these methods were not very easy to be used with Spring, especially
with its XML configuration.

Hence we introduced DaleqSupportBean in the daleq-spring. This is
a FactoryBean, which creates an instance of DaleqSupport, has
some properties to customize it and that's that. It's used as
spring intends it to be used. Furthermore it is not necessary
anymore to configure a SpringConnectionFactory on your own, because
the BeanFactory takes care for this now.

Therefore we removed all factory methods form DbUnitSupport.

All Spring examples have been updated with the FactoryBean approach.

## Releases prior to 0.4

As a matter of fact we did not wrote release notes prior to version 0.4.
We are sorry.
