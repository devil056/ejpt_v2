// IMPORTANT: Start your code on the 2nd line
lockPref("toolkit.telemetry.enabled",false);
pref("toolkit.telemetry.unified", false);
pref("toolkit.telemetry.server", "data:,");
pref("toolkit.telemetry.archive.enabled", false);
pref("toolkit.telemetry.newProfilePing.enabled", false);
pref("toolkit.telemetry.shutdownPingSender.enabled", false);
pref("toolkit.telemetry.updatePing.enabled", false);
pref("toolkit.telemetry.bhrPing.enabled", false);
pref("toolkit.telemetry.firstShutdownPing.enabled", false); 

pref("geo.provider.network.url", "https://location.services.mozilla.com/v1/geolocate?key=%MOZILLA_API_KEY%");
pref("geo.provider.ms-windows-location", false); // [WINDOWS]
pref("geo.provider.use_geoclue", false);

pref("datareporting.policy.dataSubmissionEnabled", false);
pref("datareporting.healthreport.uploadEnabled", false);
pref("toolkit.telemetry.coverage.opt-out", true); // [HIDDEN PREF]
pref("toolkit.coverage.opt-out", true); // [FF64+] [HIDDEN PREF]
pref("toolkit.coverage.endpoint.base", "");
pref("browser.ping-centre.telemetry", false);
pref("browser.newtabpage.activity-stream.feeds.telemetry", false);
pref("browser.newtabpage.activity-stream.telemetry", false);

pref("app.shield.optoutstudies.enabled", false);
pref("app.normandy.enabled", false);
pref("app.normandy.api_url", "");

pref("breakpad.reportURL", "");
pref("browser.tabs.crashReporting.sendReport", false);
pref("browser.crashReports.unsubmittedCheck.autoSubmit2", false);
pref("network.dns.disablePrefetch", true);
pref("network.predictor.enabled", false);
pref("network.predictor.enable-prefetch", false); 
pref("browser.download.animateNotifications",false);
pref("security.dialog_enable_delay",0);
pref("network.prefetch-next",false);
pref("toolkit.cosmeticAnimations.enabled",false);

pref("gfx.canvas.accelerated.cache-items", 4096);
pref("gfx.canvas.accelerated.cache-size", 512);
pref("gfx.content.skia-font-cache-size", 20);
pref("browser.cache.memory.capacity", 1048576);
pref("browser.cache.memory.max_entry_size", 65536);

pref("network.http.max-connections", 1800);
pref("network.http.max-persistent-connections-per-server", 10);
pref("network.http.max-urgent-start-excessive-connections-per-host", 5);
pref("network.websocket.max-connections", 400);
pref("network.http.pacing.requests.min-parallelism", 12);
pref("network.http.pacing.requests.burst", 20);
pref("network.http.connection-retry-timeout", 0);
pref("network.dnsCacheEntries", 10000);
pref("network.dnsCacheExpiration", 86400);
pref("network.dns.max_high_priority_threads", 8);
