second
======
+	
63	+	/* detect if this is a arm64 binary */
64	+	if (pvars->mh->magic == MH_MAGIC_64) {
65	+		lc = (struct load_command *)((unsigned char *)pvars->mh + sizeof(struct mach_header_64));
66	+		printf("[+] detected 64bit ARM binary in memory.\n");
67	+	} else { /* we might want to check for other errors here, too */
68	+		lc = (struct load_command *)((unsigned char *)pvars->mh + sizeof(struct mach_header));
69	+		printf("[+] detected 32bit ARM binary in memory.\n");
70	+	}
71	+	
62	72	 	/* searching all load commands for an LC_ENCRYPTION_INFO load command */
63		-	lc = (struct load_command *)((unsigned char *)pvars->mh + sizeof(struct mach_header));
64		-		
65	73	 	for (i=0; i<pvars->mh->ncmds; i++) {
66	74	 		/*printf("Load Command (%d): %08x\n", i, lc->cmd);*/
67	75	 		
68		-		if (lc->cmd == LC_ENCRYPTION_INFO) {
76	+		if (lc->cmd == LC_ENCRYPTION_INFO || lc->cmd == LC_ENCRYPTION_INFO_64) {
69	77	 			eic = (struct encryption_info_command *)lc;
70	78	 			
71	79	 			/* If this load command is present, but data is not crypted then exit */
...	...	@@ -113,7 +121,7 @@ void dumptofile(int argc, const char **argv, const char **envp, const char **app
113	121	 					printf("[-] Could not find correct arch in FAT image\n");
114	122	 					_exit(1);
115	123	 				}
116		-			} else if (fh->magic == MH_MAGIC) {
124	+			} else if (fh->magic == MH_MAGIC || fh->magic == MH_MAGIC_64) {
117	125	 				printf("[+] Executable is a plain MACH-O image\n");
118	126	 			} else {
 119	127	 				printf("[-] Executable is of unknown type\n");
