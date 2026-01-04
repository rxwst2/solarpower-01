# solar_irradiance_plot.py
# ---------------------------------------------
# Plot hourly clear-sky irradiance (GHI) curves
# for any location and date using a simple model.
#
# Example: Boston, MA on February 21
# ---------------------------------------------

import numpy as np
import matplotlib.pyplot as plt


def solar_declination(day_of_year):
    """Return solar declination (radians)."""
    return np.radians(23.45 * np.sin(np.radians(360 * (284 + day_of_year) / 365)))


def extraterrestrial_irradiance(day_of_year):
    """Return extraterrestrial solar irradiance normal to the sun's rays."""
    return 1367 * (1 + 0.033 * np.cos(2 * np.pi * day_of_year / 365))


def air_mass(theta_z):
    """Kasten-Young air mass model."""
    theta_deg = np.degrees(theta_z)
    return 1 / (np.cos(theta_z) + 0.50572 * (96.07995 - theta_deg) ** -1.6364)


def clear_sky_irradiance(latitude_deg, day_of_year):
    """Compute hourly clear-sky GHI for a given latitude and day of year."""
    phi = np.radians(latitude_deg)
    delta = solar_declination(day_of_year)
    I0 = extraterrestrial_irradiance(day_of_year)

    hours = np.arange(6, 19)  # 6 AM to 6 PM local solar time
    hour_angle = np.radians(15 * (hours - 12))

    # Solar zenith angle
    cos_theta_z = (
        np.sin(phi) * np.sin(delta)
        + np.cos(phi) * np.cos(delta) * np.cos(hour_angle)
    )
    cos_theta_z = np.clip(cos_theta_z, 0, None)
    theta_z = np.arccos(cos_theta_z)

    # Atmospheric transmittance factors
    Tb = 0.7   # beam
    Td = 0.1   # diffuse

    # Beam & diffuse components
    DNI = I0 * Tb ** (1 / np.cos(theta_z))
    DHI = I0 * Td

    GHI = DNI * cos_theta_z + DHI

    return hours, GHI


def plot_ghi(hours, ghi, title="Clear-Sky Irradiance"):
    """Plot GHI vs. hour."""
    plt.figure(figsize=(8, 5))
    plt.plot(hours, ghi, marker="o")
    plt.xlabel("Hour of Day")
    plt.ylabel("GHI (W/m²)")
    plt.title(title)
    plt.grid(True)
    plt.tight_layout()
    plt.show()


# ----------------------------------------------------
# Example Usage: Boston, MA on Feb 21 (DOY = 52)
# ----------------------------------------------------
if __name__ == "__main__":
    latitude = 42.36      # Boston
    day_of_year = 52      # Feb 21

    hours, ghi = clear_sky_irradiance(latitude, day_of_year)

    plot_ghi(
        hours,
        ghi,
        title="Clear-Sky Global Horizontal Irradiance (Boston, Feb 21)",
    )
