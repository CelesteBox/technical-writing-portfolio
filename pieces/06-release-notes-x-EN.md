### Release Notes (EN)

# Release Notes — X Platform v1.8.0

**Release date:** January 2026
**Type:** Minor release
**Impact:** Low to medium (no breaking changes)

This release focuses on performance improvements, reliability enhancements, and incremental updates to the X Platform API. No action is required for existing integrations, but teams are encouraged to review the changes below.

---

### Improvements

* **Improved webhook delivery reliability**
  Retry logic has been optimized to reduce failed deliveries caused by transient network errors.

* **Faster API response times**
  Average response latency has been reduced across core endpoints, particularly for read-heavy operations.

* **Enhanced logging and observability**
  Internal logging has been expanded to improve traceability of integration-related issues.

---

### Bug Fixes

* Fixed an issue where duplicate webhook events could be emitted under rare retry conditions.
* Resolved an edge case affecting pagination when filtering large datasets.
* Corrected inconsistent error messages returned by the authentication endpoint.

---

### API Changes

* **No breaking changes introduced.**
* Response schemas remain backward compatible.
* Minor internal adjustments were made to improve consistency across error payloads.

---

### Documentation Updates

* Clarified webhook retry behavior in the integration guide.
* Updated API reference examples for improved accuracy.
* Added troubleshooting notes related to common authentication errors.

---

### Upgrade Notes

No migration steps are required for this release.

Existing integrations will continue to function as expected. Teams may optionally review the updated documentation to take advantage of the improved webhook reliability and observability features.

---

### Known Limitations

* Webhook delivery guarantees remain *at-least-once*. Consumers should continue to handle duplicate events.
* Ordering of webhook events is not guaranteed.

---

### Feedback
If you encounter any issues related to this release or have feedback on the platform, please contact the X Platform support team.
