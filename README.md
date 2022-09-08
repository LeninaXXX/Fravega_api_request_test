### Files and things to have in mind:

 * `get_reports_in_parallel_original.py` : Where inquiries regarding Google Ads API are concentrated.
  * Queries: This is the example query (lines 46 to 54 as of this writing)
  
  ```
   # Define the GAQL query strings to run for each customer ID.
    campaign_query = """
        SELECT campaign.name, campaign.status, campaign.id, metrics.impressions, metrics.clicks
        FROM campaign
        WHERE segments.date DURING LAST_7_DAYS"""
    ad_group_query = """
        SELECT campaign.name, campaign.status, campaign.id, ad_group.id, metrics.impressions, metrics.clicks
        FROM ad_group
        WHERE segments.date DURING LAST_7_DAYS"""       # this is what decides the time period
    ```

    `DURING` GAQL parameter has the following allowed values:
    (From: https://developers.google.com/google-ads/api/docs/query/date-ranges )

    The list of valid predefined date ranges is as follows:
    Date range 	            Reports are generated for...
    ----------------------------------------------------
    TODAY 	                Today only.
    YESTERDAY 	            Yesterday only.
    LAST_7_DAYS 	        The last 7 days not including today.
    LAST_BUSINESS_WEEK 	    The 5 day business week, Monday through Friday, of the previous business week.
    THIS_MONTH 	            All days in the current month.
    LAST_MONTH 	            All days in the previous month.
    LAST_14_DAYS 	        The last 14 days not including today.
    LAST_30_DAYS 	        The last 30 days not including today.
    THIS_WEEK_SUN_TODAY 	The period between the previous Sunday and the current day.
    THIS_WEEK_MON_TODAY 	The period between the previous Monday and the current day.
    LAST_WEEK_SUN_SAT 	    The 7-day period starting with the previous Sunday.
    LAST_WEEK_MON_SUN 	    The 7-day period starting with the previous Monday.

### Migration from old-style **Google AdWords** *AdWords Query Language* (AWQL) to **Google Ads** *Google Ads Query Language* (GAQL):

(From: [Resource mappings - Keywords Performance](https://developers.google.com/google-ads/api/docs/migration/mapping#keywords_performance)

#### Keywords Performance
    
    | Old Fields - AWQL                         | New fields - GAQL
    | ----------------------------------------- |:---------------------------------------
    | AccountDescriptiveName                    | customer.descriptive_name
    | Date                                      | segments.date
    | Device                                    | segments.device
    | CampaignName                              | campaign.name
    | Criteria                                  | ad_group_criterion.keyword.text
    | AdGroupName                               | ad_group.name
    | Status                                    | ad_group_criterion.status
    | KeywordMatchType                          | ad_group_criterion.keyword.match_type
    | CpcBid                                    | ad_group_criterion.effective_cpc_bid_micros
    | Clicks                                    | metrics.clicks
    | Impressions                               | metrics.impressions
    | AverageCpc                                | metrics.average_cpc
    | Ctr                                       | metrics.ctr
    | Cost                                      | metrics.cost_micros
    | ~~AveragePosition~~	                    | **(DEPRECATED)**  
    | QualityScore                              | ad_group_criterion.quality_info.quality_score
    | Labels    			                    | REQUIRES `Select label.name from the resource ad_group_label`
    | SearchImpressionShare                     | metrics.search_impression_share
    | SearchRankLostImpressionShare             | metrics.search_rank_lost_impression_share
    | SearchExactMatchImpressionShare           | metrics.search_exact_match_impression_share
    | Conversions                               | metrics.conversions
    | AllConversions                            | metrics.all_conversions
    | CrossDeviceConversions                    | metrics.cross_device_conversions     
    | ConversionValue                           | metrics.conversions_value
    | AllConversionValue                        | metrics.all_conversions_value
    | VideoQuartile100Rate                      | metrics.video_quartile_p100_rate
    | VideoQuartile75Rate                       | metrics.video_quartile_p75_rate
    | VideoQuartile50Rate                       | metrics.video_quartile_p50_rate
    | AveragePageviews                          | metrics.average_page_views
    | VideoViews                                | metrics.video_views

#### Ad Performance
    
    | Old Fields - AWQL       | New fields - GAQL                 |
    | ----------------------- | ---------------------------------:|
    | AccountDescriptiveName  | customer.descriptive_name         |
    | Date                    | segments.date                     |
    | Device                  | segments.device                   |
    | CampaignName            | campaign.name                     |
    | AdGroupName             | ad_group.name                     |
    | Id                      | ad_group_ad.ad.id                 |
    | AdType                  | ad_group_ad.ad.type               |
    | Headline                | ad_group_ad.ad.text_ad.headline   |
    | ImageCreativeName       | ad_group_ad.ad.image_ad.name      |
    | Clicks                  | metrics.clicks                    |
    | Impressions             | metrics.impressions               |
    | Ctr                     | metrics.ctr                       |
    | AverageCpc              | metrics.average_cpc               |
    | AverageCpm              | metrics.average_cpm               |
    | Cost                    | metrics.cost_micros               |
    | ~~AveragePosition~~     | **(DEPRECATED)**                  |
    | CreativeFinalUrls       | ad_group_ad.ad.final_urls         |
    | ExternalCustomerId      | customer.id                       |
    | CreativeDestinationUrl  | (DEPRECATED)                      |
    | CreativeFinalMobileUrls | (DEPRECATED)                      |
    | Status 	              | ad_group_ad.status                |
    | Conversions             | metrics.conversions               |       
    | AllConversionValue      | metrics.all_conversions_value     |       
    | CrossDeviceConversions  | metrics.cross_device_conversions  |
    | AllConversions          | metrics.all_conversions           |
    | ConversionValue         | metrics.conversions_value         |
    | VideoQuartile100Rate    | metrics.video_quartile_p100_rate  |
    | VideoQuartile75Rate     | metrics.video_quartile_p75_rate   |
    | VideoQuartile50Rate     | metrics.video_quartile_p50_rate   |            
    | VideoViews              | metrics.video_views               |