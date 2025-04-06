from django.db import models
from django.core.validators import FileExtensionValidator

class IPOInfo(models.Model):
    """
    Model to store all IPO-related information as specified in the project requirements.
    """
    # Status choices for the IPO
    STATUS_CHOICES = (
        (0, 'Upcoming'),
        (1, 'Open'),
        (2, 'Closed'),
        (3, 'Listed'),
    )
    
    # Issue type choices
    ISSUE_TYPE_CHOICES = (
        ('main', 'Main Board IPO'),
        ('sme', 'SME IPO'),
        ('other', 'Other'),
    )
    
    company_logo = models.ImageField(upload_to='ipo_logos/', blank=True, null=True)
    company_name = models.CharField(max_length=255)
    price_band = models.CharField(max_length=100)  # e.g., "₹100-₹120"
    open_date = models.DateField()
    close_date = models.DateField()
    issue_size = models.CharField(max_length=100)  # e.g., "₹500 Cr"
    issue_type = models.CharField(max_length=10, choices=ISSUE_TYPE_CHOICES, default='main')
    listing_date = models.DateField()
    status = models.IntegerField(choices=STATUS_CHOICES, default=0)
    ipo_price = models.DecimalField(max_digits=10, decimal_places=2, null=True, blank=True)
    listing_price = models.DecimalField(max_digits=10, decimal_places=2, null=True, blank=True)
    listing_gain = models.DecimalField(max_digits=10, decimal_places=2, null=True, blank=True)
    current_market_price = models.DecimalField(max_digits=10, decimal_places=2, null=True, blank=True, verbose_name='CMP')
    current_return = models.DecimalField(max_digits=10, decimal_places=2, null=True, blank=True)
    rhp_pdf = models.FileField(
        upload_to='ipo_documents/',
        validators=[FileExtensionValidator(['pdf'])],
        null=True,
        blank=True,
        verbose_name='RHP PDF'
    )
    drhp_pdf = models.FileField(
        upload_to='ipo_documents/',
        validators=[FileExtensionValidator(['pdf'])],
        null=True,
        blank=True,
        verbose_name='DRHP PDF'
    )
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)
    
    def __str__(self):
        return f"{self.company_name} ({self.get_status_display()})"
    
    class Meta:
        verbose_name = 'IPO Information'
        verbose_name_plural = 'IPO Informations'
        ordering = ['-open_date']
